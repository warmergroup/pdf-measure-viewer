<script setup lang="ts">
import { ref, onMounted, watch, nextTick } from 'vue'

const props = defineProps<{
  file: File
}>()

// Holatlar
const canvasRef = ref<HTMLCanvasElement | null>(null)
const currentPage = ref<number>(1)
const totalPages = ref<number>(0)
const zoomLevel = ref<number>(1.0)
const isLoading = ref<boolean>(true)
const errorMessage = ref<string>('')

// O'lchov holatlari
const measurePoints = ref<{ x: number, y: number }[]>([])
const scale = ref<string>('1:1')
const distance = ref<number>(0)
const realDistance = ref<string>('')

// PDF holati
let pdfDoc: any = null
let currentRenderTask: any = null
let currentRenderTaskTimeout: number | null = null
let isRendering = ref<boolean>(false)
let pageCache = new Map() // Sahifalar keshi

// Oldindan yuklash uchun sahifalar soni
const PRELOAD_PAGES = 2

/**
 * PDF sahifasini oldindan yuklash
 */
const preloadPage = async (pageNumber: number) => {
  if (pageCache.has(pageNumber) || !pdfDoc) return
  try {
    const page = await pdfDoc.getPage(pageNumber)
    pageCache.set(pageNumber, page)
  } catch (error) {
    console.warn(`Sahifa ${pageNumber} ni oldindan yuklab bo'lmadi:`, error)
  }
}

/**
 * PDF faylni yuklash
 */
const loadPdf = async () => {
  try {
    isLoading.value = true
    errorMessage.value = ''
    pageCache.clear() // Keshni tozalash

    // PDF faylni yuklash
    const arrayBuffer = await props.file.arrayBuffer()
    const loadingTask = (window as any).pdfjsLib.getDocument({ data: arrayBuffer })

    // Yuklash jarayonini kuzatish
    loadingTask.onProgress = (progress: { loaded: number, total: number }) => {
      if (progress.total) {
        const percent = (progress.loaded / progress.total * 100).toFixed(0)
        isLoading.value = true
      }
    }

    pdfDoc = await loadingTask.promise

    totalPages.value = pdfDoc.numPages
    currentPage.value = 1

    // Dastlabki render
    if (canvasRef.value) {
      const page = await pdfDoc.getPage(1)
      pageCache.set(1, page) // Birinchi sahifani keshga saqlash

      const originalViewport = page.getViewport({ scale: 1.0 })
      const container = canvasRef.value.parentElement

      if (container) {
        const containerWidth = container.clientWidth - 48 // padding hisobga olingan
        zoomLevel.value = containerWidth / originalViewport.width
      }

      // Keyingi sahifalarni oldindan yuklash
      for (let i = 2; i <= Math.min(PRELOAD_PAGES, pdfDoc.numPages); i++) {
        preloadPage(i)
      }
    }

    await renderPage()
  } catch (error: any) {
    console.error('PDF yuklash xatosi:', error)
    errorMessage.value = error?.message || 'PDF faylni yuklashda xato yuz berdi'
  } finally {
    isLoading.value = false
  }
}

/**
 * PDF sahifasini render qilish
 */
const renderPage = async () => {
  // Canvas elementini tekshirish
  if (!pdfDoc || !canvasRef.value || isRendering.value) return

  try {
    isRendering.value = true

    // Agar oldingi render task bo'lsa uni bekor qilish
    if (currentRenderTask) {
      try {
        await currentRenderTask.cancel()
      } catch (e) {
        console.log('Oldingi render bekor qilindi')
      }
      currentRenderTask = null
    }

    // PDF sahifasini keshdan olish yoki yuklab olish
    let page = pageCache.get(currentPage.value)
    if (!page) {
      page = await pdfDoc.getPage(currentPage.value)
      pageCache.set(currentPage.value, page)
    }

    // Canvas va container elementlarini olish
    const canvas = canvasRef.value
    const container = canvas.parentElement
    if (!container) return

    // Container o'lchamlarini olish
    const containerStyle = window.getComputedStyle(container)
    const paddingLeft = parseInt(containerStyle.paddingLeft) || 0
    const paddingRight = parseInt(containerStyle.paddingRight) || 0
    const containerWidth = container.clientWidth - paddingLeft - paddingRight

    // PDF original o'lchamlarini olish
    const originalViewport = page.getViewport({ scale: 1.0 })

    // Yuqori sifatli rendering uchun scale ni oshirish
    const qualityScale = 2.0 // 2x sifat
    const finalScale = zoomLevel.value * qualityScale

    // PDF ni yuqori sifatda render qilish
    const viewport = page.getViewport({ scale: finalScale })

    const context = canvas.getContext('2d')
    if (!context) return

    // Canvas o'lchamlarini o'rnatish
    canvas.height = viewport.height
    canvas.width = viewport.width

    // Canvas style o'lchamlarini o'rnatish (display uchun)
    canvas.style.height = (viewport.height / qualityScale) + 'px'
    canvas.style.width = (viewport.width / qualityScale) + 'px'

    // Canvas ni tozalash
    context.clearRect(0, 0, canvas.width, canvas.height)

    // Rendering sifatini oshirish
    context.imageSmoothingEnabled = true
    context.imageSmoothingQuality = 'high'

    // Yangi render taskni yaratish
    currentRenderTask = page.render({
      canvasContext: context,
      viewport: viewport
    })

    try {
      // Render jarayonini kutish
      await currentRenderTask.promise

      // Render tugagandan so'ng o'lchov nuqtalarini chizish
      await nextTick()
      if (measurePoints.value.length > 0) {
        drawMeasurementPoints()
      }

    } catch (error: any) {
      if (error?.message !== 'Rendering cancelled') {
        console.error('Sahifa render xatosi:', error)
        errorMessage.value = 'Sahifani ko\'rsatishda xato yuz berdi'
      }
    }
  } catch (error: any) {
    console.error('Sahifa render xatosi:', error)
    errorMessage.value = 'Sahifani ko\'rsatishda xato yuz berdi'
  } finally {
    isRendering.value = false
  }
}

/**
 * Keyingi sahifaga o'tish va oldindan yuklash
 */
const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    const newPage = currentPage.value + 1
    currentPage.value = newPage

    // Keyingi sahifalarni oldindan yuklash
    for (let i = newPage + 1; i <= Math.min(newPage + PRELOAD_PAGES, totalPages.value); i++) {
      preloadPage(i)
    }

    renderPage()
  }
}

/**
 * Oldingi sahifaga o'tish va oldindan yuklash
 */
const prevPage = () => {
  if (currentPage.value > 1) {
    const newPage = currentPage.value - 1
    currentPage.value = newPage

    // Oldingi sahifalarni oldindan yuklash
    for (let i = Math.max(1, newPage - PRELOAD_PAGES); i < newPage; i++) {
      preloadPage(i)
    }

    renderPage()
  }
}

/**
 * Zoom darajasini oshirish
 */
const zoomIn = () => {
  if (zoomLevel.value < 3.0) {
    zoomLevel.value = Math.round((zoomLevel.value + 0.2) * 10) / 10
    renderPage()
  }
}

/**
 * Zoom darajasini kamaytirish
 */
const zoomOut = () => {
  if (zoomLevel.value > 0.1) {
    zoomLevel.value = Math.round((zoomLevel.value - 0.2) * 10) / 10
    renderPage()
  }
}

/**
 * Canvas bosilganda o'lchov nuqtalarini qo'shish
 */
const handleCanvasClick = async (event: MouseEvent) => {
  if (!canvasRef.value) return

  const canvas = canvasRef.value
  const rect = canvas.getBoundingClientRect()

  // Mouse koordinatalarini canvas koordinatalariga o'tkazish
  const x = (event.clientX - rect.left) * (canvas.width / rect.width)
  const y = (event.clientY - rect.top) * (canvas.height / rect.height)

  // Quality scale ni hisobga olish
  const qualityScale = 2.0
  const adjustedX = x / qualityScale
  const adjustedY = y / qualityScale

  // Agar oldingi o'lchov tugallangan bo'lsa, yangi o'lchovni boshlash
  if (measurePoints.value.length === 2) {
    measurePoints.value = [{ x: adjustedX, y: adjustedY }]
    distance.value = 0
    realDistance.value = ''
  } else {
    // Ikkinchi nuqtani qo'shish
    measurePoints.value.push({ x: adjustedX, y: adjustedY })

    // Agar ikki nuqta tanlangan bo'lsa masofani hisoblash
    if (measurePoints.value.length === 2) {
      calculateDistance()
    }
  }

  // O'lchov nuqtalarini chizish (PDF ni qayta render qilmaslik)
  drawMeasurementPoints()
}

/**
 * Masofani hisoblash
 */
const calculateDistance = () => {
  if (measurePoints.value.length !== 2) return

  const [point1, point2] = measurePoints.value
  const dx = point2.x - point1.x
  const dy = point2.y - point1.y

  // Piksel masofani hisoblash
  distance.value = Math.round(Math.sqrt(dx * dx + dy * dy))

  // Masshtab asosida real masofani hisoblash
  const scaleMatch = scale.value.match(/1:(\d+)/)
  if (scaleMatch) {
    const scaleRatio = parseInt(scaleMatch[1])
    const realMillimeters = distance.value * scaleRatio

    realDistance.value = realMillimeters >= 1000
      ? `${(realMillimeters / 1000).toFixed(2)} m`
      : `${realMillimeters.toFixed(0)} mm`
  }
}

/**
 * O'lchov nuqtalarini chizish
 */
const drawMeasurementPoints = async () => {
  if (!canvasRef.value) return

  const canvas = canvasRef.value
  const context = canvas.getContext('2d')
  if (!context) return

  // Quality scale ni olish
  const qualityScale = 2.0

  // Har bir nuqta uchun
  measurePoints.value.forEach((point, index) => {
    // Nuqtani chizish (quality scale ga ko'ra)
    const scaledX = point.x * qualityScale
    const scaledY = point.y * qualityScale

    context.save()
    context.beginPath()
    context.arc(scaledX, scaledY, 5 * qualityScale, 0, 2 * Math.PI)
    context.fillStyle = index === 0 ? '#ef4444' : '#10b981'
    context.fill()
    context.strokeStyle = '#ffffff'
    context.lineWidth = 2 * qualityScale
    context.stroke()
    context.restore()
  })

  // Agar ikki nuqta tanlangan bo'lsa chiziq chizish
  if (measurePoints.value.length === 2) {
    const [point1, point2] = measurePoints.value

    // Chiziq (quality scale ga ko'ra)
    const scaledX1 = point1.x * qualityScale
    const scaledY1 = point1.y * qualityScale
    const scaledX2 = point2.x * qualityScale
    const scaledY2 = point2.y * qualityScale

    context.save()
    context.beginPath()
    context.moveTo(scaledX1, scaledY1)
    context.lineTo(scaledX2, scaledY2)
    context.strokeStyle = '#3b82f6'
    context.lineWidth = 2 * qualityScale
    context.stroke()
    context.restore()

    // Masofa matnini chizish
    const midX = (scaledX1 + scaledX2) / 2
    const midY = (scaledY1 + scaledY2) / 2
    const text = `${distance.value}px`

    context.save()
    context.font = `${14 * qualityScale}px Arial`
    context.fillStyle = 'white'
    context.textAlign = 'center'
    context.textBaseline = 'middle'

    // Matn orqa foni
    const textWidth = context.measureText(text).width + 10 * qualityScale
    context.fillRect(midX - textWidth / 2, midY - 10 * qualityScale, textWidth, 20 * qualityScale)

    // Matn
    context.fillStyle = '#000000'
    context.fillText(text, midX, midY)
    context.restore()
  }
}

/**
 * O'lchov ma'lumotlarini tozalash
 */
const clearMeasurement = () => {
  measurePoints.value = []
  distance.value = 0
  realDistance.value = ''

  // Canvas ni tozalash va PDF ni qayta chizish
  if (canvasRef.value) {
    const context = canvasRef.value.getContext('2d')
    if (context) {
      context.clearRect(0, 0, canvasRef.value.width, canvasRef.value.height)
      renderPage()
    }
  }
}

/**
 * Faqat o'lchov chiziqlarini tozalash (PDF ni qayta render qilmaslik)
 */
const clearMeasurementOverlay = () => {
  if (canvasRef.value) {
    const context = canvasRef.value.getContext('2d')
    if (context) {
      // Faqat o'lchov chiziqlarini tozalash
      const viewport = pdfDoc?.getPage(currentPage.value)?.getViewport({ scale: zoomLevel.value })
      if (viewport) {
        context.clearRect(0, 0, viewport.width, viewport.height)
      }
      // PDF ni qayta chizish
      renderPage()
    }
  }
}

/**
 * Faqat canvas overlay ni tozalash (PDF ni qayta render qilmaslik)
 */
const clearCanvasOverlay = () => {
  if (canvasRef.value) {
    const context = canvasRef.value.getContext('2d')
    if (context) {
      // Faqat o'lchov chiziqlarini tozalash
      context.clearRect(0, 0, canvasRef.value.width, canvasRef.value.height)
    }
  }
}

// Fayl o'zgarganda PDF ni qayta yuklash
watch(() => props.file, (newFile) => {
  if (newFile) {
    loadPdf()
  }
}, { immediate: true })

onMounted(() => {
  if (props.file) {
    loadPdf()
  }
})
</script>

<template>
  <div class="bg-white rounded-xl shadow-sm border border-gray-200 flex flex-col lg:flex-row">
    <!-- Toolbar - Mobile/Tablet: Top, Desktop: Left -->
    <div
      class="bg-gray-50 border-b lg:border-b-0 lg:border-r border-gray-200 p-4 lg:p-4 flex flex-row lg:flex-col gap-4 lg:gap-6 w-full lg:w-[240px]">
      <!-- Sahifa navigatsiya - Mobile/Tablet: Bottom, Desktop: Top -->
      <div class="hidden lg:block space-y-2">
        <div class="text-sm text-gray-600 font-medium mb-2">Sahifa</div>
        <div class="flex items-center justify-between px-2">
          <button @click="prevPage" :disabled="currentPage <= 1" class="btn">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"></path>
            </svg>
          </button>

          <span class="text-sm font-medium">{{ currentPage }} / {{ totalPages }}</span>

          <button @click="nextPage" :disabled="currentPage >= totalPages" class="btn">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>
            </svg>
          </button>
        </div>
      </div>

      <!-- Zoom -->
      <div class="space-y-2">
        <div class="text-sm text-gray-600 font-medium">Masshtab</div>
        <div class="flex items-center justify-between px-2">
          <button @click="zoomOut" :disabled="zoomLevel <= 0.1" class="btn-mobile lg:btn">−</button>
          <span class="text-sm font-medium">{{ Math.round(zoomLevel * 100) }}%</span>
          <button @click="zoomIn" :disabled="zoomLevel >= 3.0" class="btn-mobile lg:btn">+</button>
        </div>
      </div>

      <!-- O'lchash -->
      <div class="space-y-2">
        <div class="text-sm text-gray-600 font-medium">O'lchash</div>

        <!-- Desktop view -->
        <div class="hidden lg:flex lg:flex-col gap-2">
          <select v-model="scale" class="select w-full">
            <option value="1:1">1:1</option>
            <option value="1:10">1:10</option>
            <option value="1:50">1:50</option>
            <option value="1:100">1:100</option>
            <option value="1:200">1:200</option>
            <option value="1:500">1:500</option>
          </select>
          <button @click="clearMeasurement" class="btn w-full justify-center">
            O'lchovlarni tozalash
          </button>
        </div>

        <!-- Mobile view -->
        <div class="lg:hidden flex items-center gap-2">
          <select v-model="scale" class="select-mobile flex-1 min-w-0">
            <option value="1:1">1:1</option>
            <option value="1:10">1:10</option>
            <option value="1:50">1:50</option>
            <option value="1:100">1:100</option>
            <option value="1:200">1:200</option>
            <option value="1:500">1:500</option>
          </select>
          <button @click="clearMeasurement" class="btn-mobile !p-2 aspect-square flex-shrink-0"
            title="O'lchovlarni tozalash">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
        </div>
      </div>
    </div>

    <!-- PDF Viewer -->
    <div class="pdf-container flex-1">
      <!-- Yuklash indikatori -->
      <div v-if="isLoading" class="loading">
        <div class="loader"></div>
        <p>PDF yuklanmoqda...</p>
      </div>

      <!-- Xato xabari -->
      <div v-else-if="errorMessage" class="error">
        {{ errorMessage }}
      </div>

      <!-- PDF Canvas -->
      <canvas v-else ref="canvasRef" @click="handleCanvasClick" class="measuring"></canvas>
    </div>

    <!-- Mobile Navigation - Bottom -->
    <div class="lg:hidden bg-gray-50 border-t border-gray-200 p-4">
      <div class="flex items-center justify-between">
        <button @click="prevPage" :disabled="currentPage <= 1" class="btn-mobile">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"></path>
          </svg>
        </button>

        <span class="text-sm font-medium">{{ currentPage }} / {{ totalPages }}</span>

        <button @click="nextPage" :disabled="currentPage >= totalPages" class="btn-mobile">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.pdf-container {
  width: 100%;
  height: calc(100vh - 120px);
  min-height: 400px;
  overflow: auto;
  padding: 1.5rem;
  background: #f8fafc;
  display: flex;
  align-items: flex-start;
  justify-content: center;
}

/* Mobile responsive adjustments */
@media (max-width: 1023px) {
  .pdf-container {
    height: calc(100vh - 200px);
    padding: 1rem;
  }
}

canvas {
  background: white;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  border-radius: 0.375rem;
  width: auto;
  height: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  image-rendering: -webkit-optimize-contrast;
  image-rendering: crisp-edges;
}

.measuring {
  cursor: crosshair !important;
}

/* Desktop buttons */
.btn {
  padding: 0.5rem;
  border-radius: 0.375rem;
  border: 1px solid #e5e7eb;
  background: white;
  color: #374151;
  cursor: pointer;
  transition: all 0.2s;
}

.btn:hover:not(:disabled) {
  background: #f9fafb;
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

/* Mobile buttons */
.btn-mobile {
  padding: 0.75rem;
  border-radius: 0.5rem;
  border: 1px solid #e5e7eb;
  background: white;
  color: #374151;
  cursor: pointer;
  transition: all 0.2s;
  font-size: 0.875rem;
  font-weight: 500;
  min-width: 44px;
  min-height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.btn-mobile:hover:not(:disabled) {
  background: #f9fafb;
}

.btn-mobile:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.btn-active {
  background: #3b82f6;
  color: white;
  border-color: #2563eb;
}

.btn-active:hover {
  background: #2563eb;
}

/* Desktop select */
.select {
  padding: 0.5rem;
  border: 1px solid #e5e7eb;
  border-radius: 0.375rem;
  background: white;
}

/* Mobile select */
.select-mobile {
  padding: 0.75rem;
  border: 1px solid #e5e7eb;
  border-radius: 0.5rem;
  background: white;
  font-size: 0.875rem;
  min-height: 44px;
}

.loading,
.error {
  text-align: center;
  padding: 2rem;
  color: #6b7280;
}

.loader {
  display: inline-block;
  width: 2rem;
  height: 2rem;
  border: 3px solid #e5e7eb;
  border-top-color: #3b82f6;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

/* Responsive text sizes */
@media (max-width: 640px) {
  .text-sm {
    font-size: 0.75rem;
  }

  .font-medium {
    font-weight: 500;
  }
}

/* Touch-friendly improvements */
@media (max-width: 1023px) {
  .btn-mobile:active {
    transform: scale(0.95);
  }

  .select-mobile:focus {
    outline: 2px solid #3b82f6;
    outline-offset: 2px;
  }
}
</style>
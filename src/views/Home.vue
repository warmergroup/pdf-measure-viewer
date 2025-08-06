<script setup lang="ts">
import { ref, watch } from 'vue'
import PdfViewer from '../components/PdfViewer.vue'

// Holatlar
const selectedFile = ref<File | null>(null)
const errorMessage = ref<string>('')
const showViewer = ref<boolean>(false)

/**
 * PDF fayl yuklash funksiyasi
 * @param event - input change eventi
 */
const handleFileUpload = (event: Event) => {
    const input = event.target as HTMLInputElement
    const file = input.files?.[0]

    // Xato xabarini tozalash
    errorMessage.value = ''

    if (!file) {
        showViewer.value = false
        return
    }

    // PDF fayl formatini tekshirish
    if (file.type !== 'application/pdf') {
        errorMessage.value = 'Iltimos, faqat PDF fayl yuklang!'
        showViewer.value = false
        return
    }

    // Fayl hajmini tekshirish (50MB limit)
    if (file.size > 50 * 1024 * 1024) {
        errorMessage.value = 'Fayl hajmi juda katta! (maksimal 50MB)'
        showViewer.value = false
        return
    }

    selectedFile.value = file
    showViewer.value = true
}

/**
 * Fayl yuklash maydonini tozalash
 */
const clearFile = () => {
    selectedFile.value = null
    showViewer.value = false
    errorMessage.value = ''

    // Input ni tozalash
    const input = document.getElementById('pdf-upload') as HTMLInputElement
    if (input) {
        input.value = ''
    }
}

// Fayl o'zgarganda darhol ko'rsatish
watch(selectedFile, (newFile) => {
    if (newFile) {
        showViewer.value = true
    } else {
        showViewer.value = false
    }
}, { immediate: true })
</script>

<template>
    <div class="min-h-screen bg-gradient-to-br from-gray-50 to-gray-100">
        <!-- Header -->
        <header class="bg-white shadow-sm border-b border-gray-200">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex justify-between items-center h-16">
                    <div class="flex items-center">
                        <svg class="h-8 w-8 text-blue-600 mr-3" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z">
                            </path>
                        </svg>
                        <h1 class="text-2xl font-bold text-gray-900">Binokor</h1>
                    </div>

                    <!-- PDF fayl ma'lumotlari -->
                    <div v-if="showViewer && selectedFile" class="flex items-center">
                        <div class="mr-6 text-right">
                            <h3 class="text-sm font-medium text-gray-900 truncate max-w-xs">{{ selectedFile.name }}</h3>
                            <p class="text-xs text-gray-500">{{ Math.round(selectedFile.size / 1024) }} KB</p>
                        </div>
                        <button @click="clearFile"
                            class="inline-flex items-center px-3 py-1.5 border border-gray-300 shadow-sm text-sm font-medium rounded-md text-gray-700 bg-white hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500">
                            <svg class="w-4 h-4 mr-1.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                    d="M6 18L18 6M6 6l12 12"></path>
                            </svg>
                            Boshqa fayl
                        </button>
                    </div>
                </div>
            </div>
        </header>

        <div class="max-w-7xl mx-auto px-4 py-4">
            <!-- PDF yuklash paneli -->
            <div v-if="!showViewer" class="max-w-2xl mx-auto">
                <div class="card text-center">
                    <div class="mb-6">
                        <svg class="mx-auto h-12 w-12 text-gray-400" fill="none" stroke="currentColor"
                            viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12">
                            </path>
                        </svg>
                        <h2 class="mt-4 text-xl font-semibold text-gray-900">PDF Chizmasini Yuklang</h2>
                        <p class="mt-2 text-gray-600">Chizma faylini tanlang va uni ko'rish, kattalashtirish hamda
                            o'lchash funksiyalaridan foydalaning</p>
                    </div>

                    <div class="mb-6">
                        <label for="pdf-upload" class="cursor-pointer">
                            <span class="btn-primary inline-flex items-center">
                                <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                        d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M9 12l3 3m0 0l3-3m-3 3V9">
                                    </path>
                                </svg>
                                PDF Faylni Tanlang
                            </span>
                        </label>
                        <input id="pdf-upload" type="file" accept=".pdf,application/pdf" @change="handleFileUpload"
                            class="hidden" />
                    </div>

                    <!-- Xato xabari -->
                    <div v-if="errorMessage" class="mb-4 p-4 bg-red-50 border border-red-200 rounded-lg">
                        <div class="flex items-center">
                            <svg class="w-5 h-5 text-red-400 mr-2" fill="currentColor" viewBox="0 0 20 20">
                                <path fill-rule="evenodd"
                                    d="M10 18a8 8 0 100-16 8 8 0 000 16zM8.707 7.293a1 1 0 00-1.414 1.414L8.586 10l-1.293 1.293a1 1 0 101.414 1.414L10 11.414l1.293 1.293a1 1 0 001.414-1.414L11.414 10l1.293-1.293a1 1 0 00-1.414-1.414L10 8.586 8.707 7.293z"
                                    clip-rule="evenodd"></path>
                            </svg>
                            <span class="text-red-800 font-medium">{{ errorMessage }}</span>
                        </div>
                    </div>

                    <!-- Ma'lumotlar -->
                    <div class="text-sm text-gray-500 space-y-1">
                        <p>• Faqat PDF formatidagi fayllar qabul qilinadi</p>
                        <p>• Maksimal fayl hajmi: 50MB</p>
                        <p>• Chizmada nuqtalar orasidagi masofani o'lchash mumkin</p>
                    </div>
                </div>
            </div>

            <!-- PDF ko'ruvchi -->
            <div v-if="showViewer && selectedFile">
                <PdfViewer :file="selectedFile" />
            </div>
        </div>
    </div>
</template>

<style scoped>
.card {
    background-color: white;
    border-radius: 0.75rem;
    box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
    border: 1px solid #e5e7eb;
    padding: 2rem;
}

.btn-primary {
    display: inline-flex;
    align-items: center;
    padding: 0.5rem 1rem;
    border: 1px solid transparent;
    font-size: 0.875rem;
    font-weight: 500;
    border-radius: 0.375rem;
    color: white;
    background-color: #2563eb;
    transition: background-color 0.2s;
}

.btn-primary:hover {
    background-color: #1d4ed8;
}

.btn-primary:focus {
    outline: none;
    box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.5);
}
</style>
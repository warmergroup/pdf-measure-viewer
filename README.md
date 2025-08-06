# 📐 PDF Chizma O'lchash Dasturi

Bu Vue.js 3 da yaratilgan PDF chizmalarini ko'rish va o'lchash uchun mo'ljallangan web ilovasi. Foydalanuvchilar PDF fayllarini yuklab, ko'rish, kattalashtirish/kichiklashtirish va chizmadagi nuqtalar orasidagi masofani o'lchash imkoniyatiga ega.

## ✨ Asosiy Xususiyatlar

### 📄 PDF Ko'rish
- **Yuqori sifatli rendering** - PDF fayllar aniq va tiniq ko'rsatiladi
- **Sahifa navigatsiya** - Oldingi/keyingi sahifalarga o'tish
- **Zoom funksiyasi** - 10% dan 300% gacha kattalashtirish/kichiklashtirish
- **Darhol ko'rsatish** - PDF fayl tanlangani bilan darhol ko'rsatiladi

### 📏 O'lchash Funksiyalari
- **Nuqta belgilash** - Chizmadagi istalgan nuqtalarni belgilash
- **Masofa hisoblash** - Ikki nuqta orasidagi masofani avtomatik hisoblash
- **Piksel ko'rsatish** - Masofa piksel (px) da ko'rsatiladi
- **Masshtab hisoblash** - Turli masshtablarda real masofani hisoblash

### 🎨 Foydalanuvchi Interfeysi
- **Zamonaviy dizayn** - Tailwind CSS bilan chiroyli interfeys
- **Responsive** - Har xil qurilmalarda yaxshi ko'rinadi
- **Intuitiv** - Oson va tushunarli foydalanish

## 🚀 O'rnatish va Ishlatish

### Talablar
- Node.js (14.0 yoki undan yuqori)
- npm yoki yarn

### O'rnatish

1. **Loyihani klonlash**
```bash
git clone https://github.com/warmergroup/pdf-measure-viewer.git
cd pdf-measure
```

2. **Bog'liqlarni o'rnatish**
```bash
npm install
```

3. **Development serverini ishga tushirish**
```bash
npm run dev
```

4. **Brauzerda ochish**
```
http://localhost:5173
```

### Production uchun tayyorlash
```bash
npm run build
```

## 📋 Foydalanish Ko'rsatmalari

### 1. PDF Fayl Yuklash
- "PDF Faylni Tanlang" tugmasini bosing
- PDF formatidagi faylni tanlang (maksimal 50MB)
- Fayl avtomatik ravishda yuklanadi va ko'rsatiladi

### 2. PDF Ko'rish
- **Sahifa navigatsiya**: ← → tugmalari bilan sahifalar orasida o'tish
- **Zoom**: + - tugmalari bilan kattalashtirish/kichiklashtirish
- **Masshtab**: Turli masshtablar (1:1, 1:10, 1:50, 1:100, 1:200, 1:500)

### 3. O'lchash
- **Birinchi nuqta**: Chizmadagi birinchi nuqtani bosing (qizil nuqta)
- **Ikkinchi nuqta**: Ikkinchi nuqtani bosing (yashil nuqta)
- **Natija**: Masofa avtomatik hisoblanadi va ko'rsatiladi
- **Yangi o'lchov**: Yangi nuqta bosilganda eski chiziqlar olib tashlanadi

### 4. Tozalash
- "O'lchovlarni tozalash" tugmasi bilan barcha o'lchovlarni tozalash

## 🛠️ Texnik Ma'lumotlar

### Texnologiyalar
- **Vue.js 3** - Frontend framework
- **TypeScript** - Type safety
- **Vite** - Build tool va development server
- **Tailwind CSS** - Styling
- **PDF.js** - PDF rendering

### Loyiha Strukturasi
```
vue-project/
├── src/
│   ├── components/
│   │   └── PdfViewer.vue      # Asosiy PDF ko'ruvchi komponenti
│   ├── views/
│   │   └── Home.vue           # Bosh sahifa
│   ├── assets/
│   │   └── main.css           # Global stillar
│   ├── App.vue                # Asosiy app komponenti
│   └── main.ts                # App entry point
├── public/                    # Static fayllar
├── index.html                 # HTML template
└── package.json               # Bog'liqlar
```

### Asosiy Komponentlar

#### PdfViewer.vue
- PDF fayllarni yuklash va render qilish
- Zoom va navigatsiya funksiyalari
- O'lchash logikasi
- Canvas event handling

#### Home.vue
- Fayl yuklash interfeysi
- Xatolarni ko'rsatish
- PdfViewer komponentini boshqarish

## 🔧 Konfiguratsiya

### Tailwind CSS
```javascript
// tailwind.config.js
module.exports = {
  content: ['./src/**/*.{vue,js,ts,jsx,tsx}'],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### Vite
```javascript
// vite.config.ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
})
```

## 🐛 Xatolarni Hal Qilish

### Umumiy Muammolar

1. **PDF yuklanmaydi**
   - Fayl formatini tekshiring (faqat PDF)
   - Fayl hajmini tekshiring (50MB dan kichik)

2. **O'lchash ishlamaydi**
   - Canvas elementiga to'g'ri bosilganini tekshiring
   - Brauzer console da xatolarni ko'ring

3. **Zoom ishlamaydi**
   - Sahifa to'liq yuklangani bilan kutib turing
   - Brauzer cache ni tozalang

### Development Muammolari

1. **Tailwind CSS xatosi**
   ```bash
   npm run dev
   ```
   - `@apply` direktivalarini to'g'ri ishlatish

2. **TypeScript xatolari**
   ```bash
   npm run type-check
   ```

## 📝 O'zgartirishlar Tarixi

### v1.0.0
- ✅ PDF yuklash va ko'rish
- ✅ Zoom va navigatsiya
- ✅ O'lchash funksiyasi
- ✅ Avtomatik tozalash
- ✅ Responsive dizayn

## 🤝 Hissa Qo'shish

1. Repository ni fork qiling
2. Feature branch yarating (`git checkout -b feature/amazing-feature`)
3. O'zgarishlarni commit qiling (`git commit -m 'Add amazing feature'`)
4. Branch ni push qiling (`git push origin feature/amazing-feature`)
5. Pull Request yarating

## 📄 Litsenziya

Bu loyiha MIT litsenziyasi ostida tarqatiladi.

## 📞 Aloqa

Savollar yoki takliflar uchun:
- GitHub Issues oching
- Email: [your-email@example.com]

---

**Eslatma**: Bu dastur PDF chizmalarini ko'rish va o'lchash uchun mo'ljallangan. Muhim loyihalar uchun qo'shimcha tekshiruvlar o'tkazish tavsiya etiladi.

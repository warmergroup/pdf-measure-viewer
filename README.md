# 📐 PDF Measure Viewer

PDF Measure Viewer — bu Vue 3 + TypeScript + TailwindCSS yordamida yaratilgan ochiq manbali web ilova bo'lib, foydalanuvchilarga PDF chizmalarini ko'rish va nuqtalar orasidagi masofani o'lchash imkonini beradi.

## ✨ Asosiy Xususiyatlar

### 📄 PDF Ko'rish
- **Aniq PDF rendering** – PDF.js yordamida yuqori sifatli ko'rsatish
- **Sahifa navigatsiyasi** – oldingi/keyingi sahifaga o'tish
- **Zoom funksiyasi** – 10% dan 300% gacha
- **Instant preview** – fayl tanlangach darhol ochiladi

### 📏 O'lchash Imkoniyatlari
- **Nuqta belgilash** – birinchi va ikkinchi nuqtalarni tanlash
- **Avtomatik masofa hisoblash** – piksel (px) birligida
- **Dynamic overlay** – chizma ustiga real vaqtli chiziq va o'lchov chiqarish
- **Yangi o'lchov bilan tozalash** – yangi nuqta bosilsa, eski o'lchovlar yo'q qilinadi

### 🖥️ Interfeys
- **Tailwind CSS dizayn** – zamonaviy va minimalistik ko'rinish
- **Responsive** – har xil qurilmalarda mos ko'rinadi
- **Intuitiv UI** – oddiy va foydalanuvchi uchun qulay

## 🚀 Live Demo
🔗 **Ishchi demo**: https://pdf-measure.vercel.app/

## 🧰 Texnologiyalar

| Texnologiya | Tavsif |
|-------------|--------|
| Vue 3 | Frontend framework |
| TypeScript | Tip xavfsizligi |
| Vite | Tez va yengil dev server |
| Tailwind CSS | Utility-first CSS framework |
| PDF.js | PDF rendering engine |

## 📦 O'rnatish

### Talablar
- Node.js v14+
- npm yoki yarn

### Boshlanish

```bash
# 1. Repozitoriyani klon qiling
git clone https://github.com/warmergroup/pdf-measure-viewer.git
cd pdf-measure-viewer

# 2. Bog'liqlarni o'rnating
npm install

# 3. Development serverni ishga tushiring
npm run dev
```

🔗 **Ochiladi**: http://localhost:5173

### Production build
```bash
npm run build
```

## 🖱️ Foydalanish Qo'llanmasi

### 📁 PDF Yuklash
- Fayl tanlang (maksimum 50MB)
- Ilova avtomatik yuklab, PDF ni ko'rsatadi

### ⬅️➡️ Sahifalar Oralig'ida Harakat
- Klaviaturadagi ← va → tugmalari orqali

### 🔍 Zoom Nisbati
- + va – tugmalari yoki interfeysdagi zoom tugmalari orqali
- 10% dan 300% gacha sozlanadi

### 📐 O'lchash
- Birinchi nuqtani bosing (qizil marker)
- Ikkinchi nuqtani bosing (yashil marker)
- Masofa avtomatik ravishda ko'rsatiladi (px da)

### 🧹 Tozalash
- "O'lchovni tozalash" tugmasi orqali chizma tozalanadi

## 🗂 Loyiha Tuzilishi

```
pdf-measure-viewer/
├── public/
│   └── index.html             # HTML template
├── src/
│   ├── assets/                # Tailwind CSS, rasmlar
│   ├── components/
│   │   └── PdfViewer.vue      # Asosiy pdf va o'lchov komponenti
│   ├── views/
│   │   └── Home.vue           # Kirish interfeysi
│   ├── App.vue                # Root komponent
│   └── main.ts                # Kirish nuqtasi
├── tailwind.config.js
├── vite.config.ts
└── package.json
```

## 🔧 Konfiguratsiya

### Tailwind
```javascript
// tailwind.config.js
module.exports = {
  content: ['./src/**/*.{vue,js,ts,jsx,tsx}'],
  theme: { extend: {} },
  plugins: [],
}
```

### Vite
```typescript
// vite.config.ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
})
```

## 🐞 Muammolar va Xatoliklar

### ✅ Oddiy muammolar

| Muammo | Yechim |
|--------|--------|
| PDF ko'rinmayapti | Faqat .pdf fayl tanlang |
| Zoom ishlamaydi | Brauzerni yangilang yoki cache tozalang |
| O'lchov ishlamaydi | Sahifada PDF to'liq yuklanganiga ishonch hosil qiling |
| Fayl katta | 50MB dan kichik fayllarni tanlang |

## 🔜 Rejalashtirilgan Yangi Funksiyalar

- [ ] **Masshtab kalibrovkasi** (1:100, 1:50, real birliklar bilan)
- [ ] **Ko'p nuqtali o'lchov** (poligon yoki chiziq)
- [ ] **Saqlash va eksport** (PNG yoki JSON)
- [ ] **Qorong'i rejim** (dark mode)

## 🤝 Hissa Qo'shish

Taklifingiz bormi? Xush kelibsiz!

```bash
# Fork qiling
git checkout -b feature/my-feature
git commit -m 'Add my feature'
git push origin feature/my-feature
# Pull Request yuboring
```

## 📄 Litsenziya

Bu loyiha MIT litsenziyasi asosida taqdim etiladi. Bepul foydalaning, o'zgartiring va tarqating.

## 📬 Aloqa

Agar savollar, muammolar yoki takliflaringiz bo'lsa:

- GitHub Issues orqali murojaat qiling
- Yoki youremail@example.com ga yozing

---

Ushbu dastur PDF chizmalarni o'rganish va o'lchashni qulaylashtirish uchun mo'ljallangan. Professional texnik o'lchovlar uchun maxsus dasturiy ta'minotlar tavsiya qilinadi.

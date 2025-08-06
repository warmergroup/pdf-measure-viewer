# 📐 PDF Measure Viewer

PDF Measure Viewer — bu Vue 3 + TypeScript + TailwindCSS yordamida yaratilgan ochiq manbali web ilova bo'lib, foydalanuvchilarga PDF chizmalarini ko'rish va nuqtalar orasidagi masofani o'lchash imkonini beradi.

**English**: PDF Measure Viewer is an open-source web application built with Vue 3 + TypeScript + TailwindCSS that allows users to view PDF drawings and measure distances between points.

## ✨ Asosiy Xususiyatlar / Key Features

### 📄 PDF Ko'rish / PDF Viewing
- **Aniq PDF rendering** – PDF.js yordamida yuqori sifatli ko'rsatish
- **Sahifa navigatsiyasi** – oldingi/keyingi sahifaga o'tish
- **Zoom funksiyasi** – 10% dan 300% gacha
- **Instant preview** – fayl tanlangach darhol ochiladi

### 📏 O'lchash Imkoniyatlari / Measurement Capabilities
- **Nuqta belgilash** – birinchi va ikkinchi nuqtalarni tanlash
- **Avtomatik masofa hisoblash** – piksel (px) birligida
- **Dynamic overlay** – chizma ustiga real vaqtli chiziq va o'lchov chiqarish
- **Yangi o'lchov bilan tozalash** – yangi nuqta bosilsa, eski o'lchovlar yo'q qilinadi

### 🖥️ Interfeys / Interface
- **Tailwind CSS dizayn** – zamonaviy va minimalistik ko'rinish
- **Responsive** – har xil qurilmalarda mos ko'rinadi
- **Intuitiv UI** – oddiy va foydalanuvchi uchun qulay

## 🚀 Live Demo
🔗 **Ishchi demo**: https://pdf-measure.vercel.app/

## 🧰 Texnologiyalar / Technologies

| Texnologiya / Technology | Tavsif / Description |
|-------------|--------|
| Vue 3 | Frontend framework |
| TypeScript | Tip xavfsizligi / Type safety |
| Vite | Tez va yengil dev server / Fast dev server |
| Tailwind CSS | Utility-first CSS framework |
| PDF.js | PDF rendering engine |

## 🚀 Boshlash / Getting Started

### Talablar / Requirements
- Node.js v14+
- npm yoki yarn / npm or yarn

### O'rnatish / Installation

```bash
# 1. Repozitoriyani klon qiling / Clone the repository
git clone https://github.com/warmergroup/pdf-measure-viewer.git
cd pdf-measure-viewer

# 2. Bog'liqlarni o'rnating / Install dependencies
npm install

# 3. Development serverni ishga tushiring / Start development server
npm run dev
```

🔗 **Ochiladi / Opens at**: http://localhost:5173

### Production build
```bash
npm run build
```

## 🖱️ Foydalanish / Usage

### 📁 PDF Yuklash / PDF Upload
- Fayl tanlang (maksimum 50MB) / Select file (max 50MB)
- Ilova avtomatik yuklab, PDF ni ko'rsatadi / App automatically loads and displays PDF

### ⬅️➡️ Sahifalar Oralig'ida Harakat / Page Navigation
- Klaviaturadagi ← va → tugmalari orqali / Using keyboard ← and → keys

### 🔍 Zoom Nisbati / Zoom Ratio
- + va – tugmalari yoki interfeysdagi zoom tugmalari orqali / Using + and – keys or interface zoom buttons
- 10% dan 300% gacha sozlanadi / Adjustable from 10% to 300%

### 📐 O'lchash / Measurement
- Birinchi nuqtani bosing (qizil marker) / Click first point (red marker)
- Ikkinchi nuqtani bosing (yashil marker) / Click second point (green marker)
- Masofa avtomatik ravishda ko'rsatiladi (px da) / Distance automatically displayed (in px)

### 🧹 Tozalash / Clear
- "O'lchovni tozalash" tugmasi orqali chizma tozalanadi / Clear measurements using "Clear measurements" button

## 🔧 Texnik Ma'lumotlar / Technical Details

### Loyiha Tuzilishi / Project Structure

```
pdf-measure-viewer/
├── public/
│   └── index.html             # HTML template
├── src/
│   ├── assets/                # Tailwind CSS, rasmlar / Images
│   ├── components/
│   │   └── PdfViewer.vue      # Asosiy pdf va o'lchov komponenti / Main PDF viewer component
│   ├── views/
│   │   └── Home.vue           # Kirish interfeysi / Entry interface
│   ├── App.vue                # Root komponent / Root component
│   └── main.ts                # Kirish nuqtasi / Entry point
├── tailwind.config.js
├── vite.config.ts
└── package.json
```

### Konfiguratsiya / Configuration

#### Tailwind
```javascript
// tailwind.config.js
module.exports = {
  content: ['./src/**/*.{vue,js,ts,jsx,tsx}'],
  theme: { extend: {} },
  plugins: [],
}
```

#### Vite
```typescript
// vite.config.ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
})
```

## 🐞 Xatoliklar / Troubleshooting

### ✅ Oddiy muammolar / Common Issues

| Muammo / Issue | Yechim / Solution |
|--------|--------|
| PDF ko'rinmayapti / PDF not visible | Faqat .pdf fayl tanlang / Select only .pdf files |
| Zoom ishlamaydi / Zoom not working | Brauzerni yangilang yoki cache tozalang / Refresh browser or clear cache |
| O'lchov ishlamaydi / Measurement not working | Sahifada PDF to'liq yuklanganiga ishonch hosil qiling / Ensure PDF is fully loaded |
| Fayl katta / File too large | 50MB dan kichik fayllarni tanlang / Select files smaller than 50MB |

### Development muammolari / Development Issues

1. **Tailwind CSS xatosi / Tailwind CSS error**
   ```bash
   npm run dev
   ```
   - `@apply` direktivalarini to'g'ri ishlatish / Use `@apply` directives correctly

2. **TypeScript xatolari / TypeScript errors**
   ```bash
   npm run type-check
   ```

## 🔜 Rejalashtirilgan Yangi Funksiyalar / Planned Features

- [ ] **Masshtab kalibrovkasi** (1:100, 1:50, real birliklar bilan) / **Scale calibration** (1:100, 1:50, with real units)
- [ ] **Ko'p nuqtali o'lchov** (poligon yoki chiziq) / **Multi-point measurement** (polygon or line)
- [ ] **Saqlash va eksport** (PNG yoki JSON) / **Save and export** (PNG or JSON)
- [ ] **Qorong'i rejim** (dark mode) / **Dark mode**

## 🤝 Hissa Qo'shish / Contributing

Taklifingiz bormi? Xush kelibsiz! / Have suggestions? Welcome!

```bash
# Fork qiling / Fork the repository
git checkout -b feature/my-feature
git commit -m 'Add my feature'
git push origin feature/my-feature
# Pull Request yuboring / Submit Pull Request
```

## 📄 Litsenziya / License

Bu loyiha MIT litsenziyasi asosida taqdim etiladi. Bepul foydalaning, o'zgartiring va tarqating.

This project is provided under MIT license. Use freely, modify and distribute.

## 📬 Aloqa / Contact

Agar savollar, muammolar yoki takliflaringiz bo'lsa / If you have questions, issues or suggestions:

- GitHub Issues orqali murojaat qiling / Contact via GitHub Issues
- ulugbekjonsultonov4@gmail.com ga yoki zetcoder@mail.ru ga yozing / Write to ulugbekjonsultonov4@gmail.com or zetcoder@mail.ru

---

Ushbu dastur PDF chizmalarni o'rganish va o'lchashni qulaylashtirish uchun mo'ljallangan. Professional texnik o'lchovlar uchun maxsus dasturiy ta'minotlar tavsiya qilinadi.

This application is designed to facilitate the study and measurement of PDF drawings. Special software is recommended for professional technical measurements.

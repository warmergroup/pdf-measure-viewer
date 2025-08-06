# 📐 PDF Measure Viewer

PDF Measure Viewer is an open-source web application built with Vue 3 + TypeScript + TailwindCSS that allows users to view PDF drawings and measure distances between points.

**O'zbekcha**: PDF Measure Viewer — bu Vue 3 + TypeScript + TailwindCSS yordamida yaratilgan ochiq manbali web ilova bo'lib, foydalanuvchilarga PDF chizmalarini ko'rish va nuqtalar orasidagi masofani o'lchash imkonini beradi.

## ✨ Key Features / Asosiy Xususiyatlar

### 📄 PDF Viewing / PDF Ko'rish
- **High-quality PDF rendering** – High-quality display using PDF.js
- **Page navigation** – Navigate between previous/next pages
- **Zoom functionality** – From 10% to 300%
- **Instant preview** – File opens immediately after selection

### 📏 Measurement Capabilities / O'lchash Imkoniyatlari
- **Point marking** – Select first and second points
- **Automatic distance calculation** – In pixels (px)
- **Dynamic overlay** – Real-time line and measurement display on drawing
- **Clear with new measurement** – Old measurements are removed when new point is clicked

### 🖥️ Interface / Interfeys
- **Tailwind CSS design** – Modern and minimalistic appearance
- **Responsive** – Adapts well to different devices
- **Intuitive UI** – Simple and user-friendly

## 🚀 Live Demo
🔗 **Demo**: https://pdf-measure.vercel.app/

## 🧰 Technologies / Texnologiyalar

| Technology / Texnologiya | Description / Tavsif |
|-------------|--------|
| Vue 3 | Frontend framework |
| TypeScript | Type safety / Tip xavfsizligi |
| Vite | Fast dev server / Tez va yengil dev server |
| Tailwind CSS | Utility-first CSS framework |
| PDF.js | PDF rendering engine |

## 🚀 Getting Started / Boshlash

### Requirements / Talablar
- Node.js v14+
- npm or yarn / npm yoki yarn

### Installation / O'rnatish

```bash
# 1. Clone the repository / Repozitoriyani klon qiling
git clone https://github.com/warmergroup/pdf-measure-viewer.git
cd pdf-measure-viewer

# 2. Install dependencies / Bog'liqlarni o'rnating
npm install

# 3. Start development server / Development serverni ishga tushiring
npm run dev
```

🔗 **Opens at / Ochiladi**: http://localhost:5173

### Production build
```bash
npm run build
```

## 🖱️ Usage / Foydalanish

### 📁 PDF Upload / PDF Yuklash
- Select file (max 50MB) / Fayl tanlang (maksimum 50MB)
- App automatically loads and displays PDF / Ilova avtomatik yuklab, PDF ni ko'rsatadi

### ⬅️➡️ Page Navigation / Sahifalar Oralig'ida Harakat
- Using keyboard ← and → keys / Klaviaturadagi ← va → tugmalari orqali

### 🔍 Zoom Ratio / Zoom Nisbati
- Using + and – keys or interface zoom buttons / + va – tugmalari yoki interfeysdagi zoom tugmalari orqali
- Adjustable from 10% to 300% / 10% dan 300% gacha sozlanadi

### 📐 Measurement / O'lchash
- Click first point (red marker) / Birinchi nuqtani bosing (qizil marker)
- Click second point (green marker) / Ikkinchi nuqtani bosing (yashil marker)
- Distance automatically displayed (in px) / Masofa avtomatik ravishda ko'rsatiladi (px da)

### 🧹 Clear / Tozalash
- Clear measurements using "Clear measurements" button / "O'lchovni tozalash" tugmasi orqali chizma tozalanadi

## 🔧 Technical Details / Texnik Ma'lumotlar

### Project Structure / Loyiha Tuzilishi

```
pdf-measure-viewer/
├── public/
│   └── index.html             # HTML template
├── src/
│   ├── assets/                # Tailwind CSS, images / Rasmlar
│   ├── components/
│   │   └── PdfViewer.vue      # Main PDF viewer component / Asosiy pdf va o'lchov komponenti
│   ├── views/
│   │   └── Home.vue           # Entry interface / Kirish interfeysi
│   ├── App.vue                # Root component / Root komponent
│   └── main.ts                # Entry point / Kirish nuqtasi
├── tailwind.config.js
├── vite.config.ts
└── package.json
```

### Configuration / Konfiguratsiya

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

## 🐞 Troubleshooting / Xatoliklar

### ✅ Common Issues / Oddiy muammolar

| Issue / Muammo | Solution / Yechim |
|--------|--------|
| PDF not visible / PDF ko'rinmayapti | Select only .pdf files / Faqat .pdf fayl tanlang |
| Zoom not working / Zoom ishlamaydi | Refresh browser or clear cache / Brauzerni yangilang yoki cache tozalang |
| Measurement not working / O'lchov ishlamaydi | Ensure PDF is fully loaded / Sahifada PDF to'liq yuklanganiga ishonch hosil qiling |
| File too large / Fayl katta | Select files smaller than 50MB / 50MB dan kichik fayllarni tanlang |

### Development Issues / Development muammolari

1. **Tailwind CSS error / Tailwind CSS xatosi**
   ```bash
   npm run dev
   ```
   - Use `@apply` directives correctly / `@apply` direktivalarini to'g'ri ishlatish

2. **TypeScript errors / TypeScript xatolari**
   ```bash
   npm run type-check
   ```

## 🔜 Planned Features / Rejalashtirilgan Yangi Funksiyalar

- [ ] **Scale calibration** (1:100, 1:50, with real units) / **Masshtab kalibrovkasi** (1:100, 1:50, real birliklar bilan)
- [ ] **Multi-point measurement** (polygon or line) / **Ko'p nuqtali o'lchov** (poligon yoki chiziq)
- [ ] **Save and export** (PNG or JSON) / **Saqlash va eksport** (PNG yoki JSON)
- [ ] **Dark mode** / **Qorong'i rejim**

## 🤝 Contributing / Hissa Qo'shish

Have suggestions? Welcome! / Taklifingiz bormi? Xush kelibsiz!

```bash
# Fork the repository / Fork qiling
git checkout -b feature/my-feature
git commit -m 'Add my feature'
git push origin feature/my-feature
# Submit Pull Request / Pull Request yuboring
```

## 📄 License / Litsenziya

This project is provided under MIT license. Use freely, modify and distribute.

Bu loyiha MIT litsenziyasi asosida taqdim etiladi. Bepul foydalaning, o'zgartiring va tarqating.

## 📬 Contact / Aloqa

If you have questions, issues or suggestions / Agar savollar, muammolar yoki takliflaringiz bo'lsa:

- Contact via GitHub Issues / GitHub Issues orqali murojaat qiling
- Write to ulugbekjonsultonov4@gmail.com or zetcoder@mail.ru
- ulugbekjonsultonov4@gmail.com ga yoki zetcoder@mail.ru ga yozing

---

This application is designed to facilitate the study and measurement of PDF drawings. Special software is recommended for professional technical measurements.

Ushbu dastur PDF chizmalarni o'rganish va o'lchashni qulaylashtirish uchun mo'ljallangan. Professional texnik o'lchovlar uchun maxsus dasturiy ta'minotlar tavsiya qilinadi.

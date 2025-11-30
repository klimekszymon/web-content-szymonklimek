# Optymalizacja prędkości strony internetowej

Szybka strona to podstawa UX i SEO. Oto kluczowe techniki przyspieszające ładowanie:

## 1. Optymalizacja obrazów

- **WebP/AVIF** zamiast JPEG/PNG
- **Lazy loading**: `loading="lazy"`
- Kompresja: TinyPNG, ImageOptim

```html
<img src="image.webp" loading="lazy" width="800" height="600" alt="Opis">
```

## 2. Minimalizacja CSS/JS

| Technika | Redukcja rozmiaru | Narzędzie |
|----------|------------------|-----------|
| Minifikacja | 30-50% | Terser, cssnano |
| Tree shaking | 40-70% | Webpack, Vite |
| Critical CSS | 80% szybszy FCP | Critical |

## 3. Core Web Vitals

- **LCP < 2.5s**: Largest Contentful Paint
- **FID < 100ms**: First Input Delay
- **CLS < 0.1**: Cumulative Layout Shift

## 4. Narzędzia pomiaru

- **Lighthouse** (Chrome DevTools)
- **PageSpeed Insights** (Google)
- **WebPageTest** (szczegółowe)

## 5. CDN i cachowanie

```html
<!-- Service Worker -->
<link rel="preload" href="/critical.css" as="style">
<meta http-equiv="Cache-Control" content="max-age=31536000">
```

## Wyniki optymalizacji

**Przed**: 8.2s → **Po**: 1.9s (76% poprawa)

**Szybkość = konwersje + SEO + UX.**
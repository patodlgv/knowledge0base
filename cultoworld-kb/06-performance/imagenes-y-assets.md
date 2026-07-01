# Imágenes, assets y code splitting

> **Tier: AMBOS** · Las imágenes son ~50% del peso de una página típica. Es la palanca #1 del <2s.

## Formatos: AVIF > WebP > JPG

- **AVIF**: ~50% más chico que JPG a igual calidad. Soporte universal moderno.
- **WebP**: fallback sólido, ~30% mejor que JPG.
- SVG para logos/iconos/patrones (y se puede animar con GSAP).
- PNG solo para alpha si AVIF/WebP con alpha no aplican (raro).

## Next.js: next/image lo hace todo (fábrica)

```tsx
import Image from "next/image";

// Hero (el LCP):
<Image src="/hero.jpg" alt="Interior del restaurante" width={1600} height={1000}
  priority sizes="100vw" />
// priority = fetchpriority high + sin lazy. SOLO en el hero.

// Resto (lazy automático):
<Image src="/plato.jpg" alt="Tacos de la casa" width={800} height={600}
  sizes="(max-width: 768px) 100vw, 33vw" />
```

next/image genera AVIF/WebP, `srcset` y lazy load solo. Regla: `sizes` correcto o servirá tamaños de más (el default asume 100vw).

## Vanilla/Vite (premium y demos sueltos)

```html
<picture>
  <source srcset="hero-800.avif 800w, hero-1600.avif 1600w" type="image/avif">
  <source srcset="hero-800.webp 800w, hero-1600.webp 1600w" type="image/webp">
  <img src="hero-1600.jpg" srcset="hero-800.jpg 800w, hero-1600.jpg 1600w"
       sizes="100vw" width="1600" height="1000" alt="…" fetchpriority="high">
</picture>
```

Pipeline offline con sharp:

```js
// optimize-images.mjs — npm i sharp
import sharp from "sharp";
import { readdir } from "fs/promises";

for (const f of await readdir("src/img/raw")) {
  const name = f.replace(/\.\w+$/, "");
  for (const w of [800, 1600]) {
    await sharp(`src/img/raw/${f}`).resize(w).avif({ quality: 55 }).toFile(`public/img/${name}-${w}.avif`);
    await sharp(`src/img/raw/${f}`).resize(w).webp({ quality: 70 }).toFile(`public/img/${name}-${w}.webp`);
  }
}
```

Calidades de partida: AVIF 50–60, WebP 68–75 (bajar hasta que duela, subir uno).

## Lazy loading correcto

```html
<img loading="lazy" decoding="async" …>   <!-- todo lo bajo el fold -->
```
- El hero JAMÁS lleva `loading="lazy"`.
- Lo que está apenas bajo el fold: mejor eager que lazy (el lazy de lo casi-visible retrasa su pintura).
- `IntersectionObserver` con `rootMargin: "200px"` para lazy de componentes caros (mapas, videos, canvas).

## Video (la trampa del tier ligero)

- Hero de video en ligero: evitar. Si el cliente insiste: MP4/WebM < 1 MB, 720p, sin audio, `preload="none"` + `poster` (el poster es el LCP), solo desktop.
- Embeds de YouTube: fachada (imagen + botón play que inyecta el iframe al click) — el iframe real pesa ~1 MB de JS.

## Code splitting

### Next.js (fábrica)
- Automático por ruta. Lo importante: **mantener client components pequeños y hoja** ("use client" en el componente animado, no en la página entera).
- Componentes pesados bajo demanda:
```tsx
import dynamic from "next/dynamic";
const Mapa = dynamic(() => import("./Mapa"), { ssr: false, loading: () => <div className="skeleton" style={{aspectRatio: "16/9"}} /> });
```
- Third-parties con `next/script strategy="lazyOnload"`.

### Vite (premium)
```js
// import estático solo lo del primer paint; el resto dinámico:
const { initScene } = await import("./scene.js");     // chunk aparte automático
```
```js
// vite.config.js — vendor split para cache estable
export default {
  build: { rollupOptions: { output: { manualChunks: { three: ["three"], gsap: ["gsap"] } } } }
};
```

## Qué mata el performance en móvil (lista negra)

1. Imagen hero sin optimizar (JPG 2 MB) — el asesino #1
2. `loading="lazy"` en el LCP
3. Chat widgets / pixels cargados de inmediato
4. Fuentes: 4+ pesos, TTF sin subset
5. Video autoplay en móvil
6. Librerías redundantes (GSAP + Framer Motion + AOS…)
7. `background-attachment: fixed` (iOS lo rompe y fuerza repaints)
8. Sombras/filtros animados por frame (`filter: blur` en scroll)
9. Canvas/WebGL en el tier equivocado
10. Hidratación de páginas enteras marcadas "use client"

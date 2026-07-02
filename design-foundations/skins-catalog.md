
# 🗃️ Catálogo de Skins — Combinaciones Probadas por Industria

> El "almacén" de combinaciones color+tipografía VALIDADAS. No se llena de un tiro
> ni con relleno inventado: cada skin entra cuando se prueba en un proyecto real,
> se valida contraste (WCAG) y soporte de ñ/tildes. Así el catálogo crece con
> calidad. Meta: rumbo a 100+ skins clasificadas por industria, construidas, no
> fabricadas de memoria.
>
> Cómo se usa: cliente nuevo → buscas su industria aquí → si hay skin, la usas como
> punto de partida y la ajustas; si no, la creas (con `color-system.md` +
> `typography-system.md` + `color-typography-resources.md`) y la registras abajo.

---

## 📋 Formato de cada skin
```
### [Nombre] — [Industria]
- Tier/uso recomendado: A / A+C / B
- Fondo (60%): --color-bg = [hex / token]
- Secundario (30%): --color-surface = [hex]
- Acento (10%): --color-brand = [hex]
- Texto: --color-text = [hex]  (contraste verificado: __:1 ✅)
- Display: [fuente]  | Cuerpo: [fuente]
- Modo: claro / oscuro / ambos
- ñ/tildes: ✅
- Validado en: [proyecto/cliente]
- Notas: [por qué funciona, para qué mood]
```

---

## ✅ Skins registradas

### Culto — Estudio / Agencia premium
- Tier/uso: B (Marca/WebGL) — también sirve A para landings premium
- Fondo (60%): `--color-bg` = #0A1128 (navy-900)
- Secundario (30%): `--color-surface` = #1B2A4A (navy-700)
- Acento (10%): `--color-brand` = #C5A46D (gold-500)
- Texto: #F6F4F1 (ivory) — contraste verificado: alto ✅
- Display: Playfair Display | Cuerpo: Inter
- Modo: oscuro (principal)
- ñ/tildes: ✅
- Validado en: sitio Culto (hero WebGL del monumento)
- Notas: navy + oro + ivory = combinación de lujo clásica. El oro como acento
  escaso funciona perfecto para CTAs. Vibra premium/sofisticada.

---

## 🎯 Industrias a cubrir (checklist — se llena construyendo)

> Prioriza las más comunes en México que necesitan página. Marca ✅ al registrar.

**Las más comunes / que más venden:**
- [ ] Restaurantes / cafeterías
- [ ] Salud / clínicas / dentistas
- [ ] Belleza / estética / spa / barbería
- [ ] Inmobiliarias / bienes raíces
- [ ] Despachos legales / contables
- [ ] Consultorías / coaching
- [ ] Gimnasios / fitness / wellness
- [ ] Tiendas / e-commerce / boutiques
- [ ] Constructoras / arquitectura
- [ ] Talleres / automotriz
- [ ] Eventos / bodas / fotografía
- [ ] Educación / cursos / academias
- [ ] Tecnología / SaaS / startups
- [ ] Agencias / estudios creativos ✅ (Culto)
- [ ] Turismo / hoteles / Airbnb
- [ ] Logística / transporte
- [ ] Agro / alimentos / productores
- [ ] Moda / ropa / diseñadores
- [ ] Finanzas / seguros / fintech
- [ ] Entretenimiento / música / artistas

> Cada proyecto real suma 1-2 skins. En 2-3 meses, decenas reales y clasificadas.

---

## ðŸ“¦ Lote base cultoworld-kb (julio 2026)

> 9 skins derivadas de `cultoworld-kb/04-color/paletas-por-nicho.md` + `05-tipografia/pares-por-negocio.md`. Contraste WCAG verificado en los pares principales; psicologÃ­a del color documentada en esos archivos. Estado: **base propuesta** â€” se marcan "Validado en: [proyecto]" cuando se usen en un demo/cliente real.

### Confianza ClÃ­nica â€” Salud / dental
- Tier/uso: A / A+C
- Fondo (60%): --color-bg = #f8fafb Â· Secundario (30%): --color-surface = #e8f1f2
- Acento (10%): --color-brand = #0e7c86 (teal) Â· Texto: --color-text = #16303f (âœ… 4.5:1+)
- Display: Fraunces | Cuerpo: Inter Â· Modo: claro Â· Ã±/tildes: âœ…
- Notas: teal mÃ©dico = limpieza+competencia sin frialdad; evita rojos. Alt display: Source Serif 4.

### SazÃ³n de Casa â€” Restaurante / cafÃ©
- Tier/uso: A
- Fondo: #faf5ef (crema) Â· Secundario: #f1e6d8
- Acento: #c0392b (rojo terracota, CTA/reservar) Â· Texto: #2b1d16 (cafÃ© espresso âœ…)
- Display: Playfair Display | Cuerpo: Lato Â· Modo: claro Â· Ã±/tildes: âœ…
- Notas: rojos/tierras estimulan apetito; azul lo suprime (evitarlo). Variante fine-dining: fondo #1a1512 + acento oro #c5a46d.

### Voltaje â€” Gym / crossfit
- Tier/uso: A
- Fondo: #101014 (negro azulado) Â· Secundario: #1b1b22
- Acento: #d7fd3a (lima elÃ©ctrico) Â· Texto: #f2f2f2 (âœ…; texto SOBRE lima = #101014)
- Display: Archivo Expanded/Black (caps, letter-spacing 0.02em) | Cuerpo: Inter Â· Modo: oscuro Â· Ã±/tildes: âœ…
- Notas: negro=poder, lima=adrenalina. Headline gigante + antes/despuÃ©s manda en este nicho.

### Navaja ClÃ¡sica â€” BarberÃ­a
- Tier/uso: A
- Fondo: #17130f (negro cafÃ©) Â· Secundario: #241d16
- Acento: #b3702c (cobre) Â· Texto: #ece7df (âœ…)
- Display: Bricolage Grotesque | Cuerpo: Inter Â· Modo: oscuro Â· Ã±/tildes: âœ…
- Notas: herencia+actitud; el cobre evoca navaja/cuero. Alt display con mÃ¡s drama: Abril Fatface.

### Patrimonio â€” Bienes raÃ­ces / hotel boutique
- Tier/uso: A / A+C (comparte ADN con la skin Culto)
- Fondo: #f6f4f1 (ivory) Â· Secundario: #eae5dd
- Acento: #b08d4f (oro viejo â€” solo CTA/detalles, NUNCA texto chico sobre claro) Â· Texto: #0f1b33 (navy âœ… 16:1)
- Display: Cormorant Garamond 600 (usar GRANDE) | Cuerpo: Inter Â· Modo: claro Â· Ã±/tildes: âœ…
- Notas: navy=estabilidad financiera, oro=exclusividad. La inversa (fondo navy) para hero/footer.

### Salvia â€” Spa / estÃ©tica
- Tier/uso: A
- Fondo: #f7f5f0 Â· Secundario: #e9ebe2 (salvia pÃ¡lido)
- Acento: #55624b (salvia profundo para botones con texto chico, 5.1:1 âœ…; #7d8c6f solo en texto grande/decorativo)
- Texto: #3a4238 (âœ…) Â· Display: Marcellus | Cuerpo: Karla Â· Modo: claro Â· Ã±/tildes: âœ…
- Notas: el error #1 del nicho es gris-claro ilegible â€” esta skin lo evita de fÃ¡brica.

### Toga â€” Legal / contable
- Tier/uso: A / A+C
- Fondo: #f5f5f3 Â· Secundario: #e8e9e6
- Acento: #7c2d3e (borgoÃ±a) Â· Texto: #1c2430 (âœ…)
- Display: Libre Caslon Text | Cuerpo: IBM Plex Sans Â· Modo: claro Â· Ã±/tildes: âœ…
- Notas: Caslon=tradiciÃ³n jurÃ­dica; borgoÃ±a=tradiciÃ³n sin frivolidad. Nada juguetÃ³n. Alt: IBM Plex 600/400 monofamilia.

### Circuito â€” Fintech / SaaS local
- Tier/uso: A / A+C / D
- Fondo: #fbfbfd Â· Secundario: #eef0f6
- Acento: #2545d3 (azul elÃ©ctrico) Â· Texto: #10141f (âœ…)
- Display: Space Grotesk (o Inter 700 tight -0.03em) | Cuerpo: Inter Â· Modo: claro (oscuro disponible) Â· Ã±/tildes: âœ…
- Notas: acento mono JetBrains Mono para cifras/labels. PrecisiÃ³n sin frialdad.

### Recreo â€” EducaciÃ³n / academias
- Tier/uso: A / A+C
- Fondo: #fdfcf8 Â· Secundario: #f0ede2
- Acento: #e8632c (naranja cÃ¡lido) Â· Texto: #232838 (âœ…)
- Display: Bricolage Grotesque | Cuerpo: Source Sans 3 Â· Modo: claro Â· Ã±/tildes: âœ…
- Notas: optimismo sin infantilizar. Funciona de kinder a cursos para adultos ajustando fotografÃ­a.

> Reglas transversales de todas: nunca #000/#fff puros Â· neutros teÃ±idos hacia el mood Â· hover = Â±8% lightness (color-mix), no hue nuevo Â· acento 1-2 veces por pantalla Â· verificar el par muted/fondo (el que mÃ¡s falla). Escala completa desde cualquier acento: `cultoworld-kb/04-color/accesibilidad-y-escalas.md` (OKLCH).


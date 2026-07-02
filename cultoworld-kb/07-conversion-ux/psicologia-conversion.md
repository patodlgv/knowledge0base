# Psicología y economía conductual de la conversión

> **Tier: AMBOS** · El "por qué" detrás de todo `07-conversion-ux/`. Fuentes: Cialdini, Kahneman/Tversky, Stanford Web Credibility, NN/g, CXL. Regla rectora: **persuasión es alinear el diseño con una verdad; dark pattern es diseñar una mentira** (y ya cuesta multas: Booking, €413M en España por urgencia manipulativa).

## Los 7 principios de Cialdini → elemento web concreto

| Principio | Implementación web | Dark pattern (NO) |
|---|---|---|
| **Reciprocidad** | Lead magnet valioso ANTES de pedir datos (cotización/diagnóstico gratis, calculadora) | "Gratis" que pide tarjeta |
| **Compromiso** | Multi-step que abre con pregunta fácil; quiz "encuentra tu plan" | Fácil entrar, imposible cancelar |
| **Prueba social** | Específica: "34% más ventas para X" > "excelente servicio". Informativa ("el más popular") para decisiones ambiguas; normativa (identidad) para marcas/moda | Reseñas falsas, "5 personas viendo esto" inventado (11% de e-commerce los usa — Princeton) |
| **Autoridad** | Credenciales visibles, cédula, prensa, casos CON metodología | Sellos no verificables |
| **Simpatía** | Caras REALES del equipo (el stock decorativo se ignora — NN/g), tono humano | — |
| **Escasez** | Solo REAL: cupos, agenda, unidades. Deadline concreto | ~40% de countdown timers auditados eran falsos; multas |
| **Unidad** | Identidad compartida: "hecho en México", comunidad, co-creación. El más fuerte cuando existe | Tribalismo fabricado |

## Sesgos aplicables (definición → uso web → límite)

- **Anclaje**: el primer número sesga todo → plan caro primero / precio tachado / "proyectos desde $X". Límite: anclas absurdas destruyen credibilidad (y los precios de referencia falsos son ilegales — PROFECO).
- **Aversión a la pérdida** (perder duele ~2×): copy de pérdida honesta ("estás perdiendo $X/mes"), garantías que transfieren el riesgo. NO confirmshaming ("No, prefiero seguir perdiendo dinero").
- **Decoy**: el tier inferior se diseña para que el medio sea obvio (Economist: 32%→84% eligió la combinada con señuelo presente).
- **Paradoja de elección**: 1 CTA primario por pantalla, 3-4 planes, nav de 5-7. Matiz honesto: el efecto es condicional (meta-análisis Scheibehenne ≈0 en promedio); aparece con opciones difíciles de comparar y sin default → la solución es default + "más popular", no amputar catálogo.
- **Dotación**: trials sin fricción, configuradores, "tu plan" — cancelar se siente perder.
- **Descuento hiperbólico**: "acceso inmediato", victoria rápida en onboarding, "resultados la primera semana" (solo si es verdad).
- **Halo**: bonito = confiable = competente. El diseño ES el argumento (ver Confianza abajo). Un typo activa el efecto inverso.
- **Fluency**: lo fácil de procesar se percibe más verdadero → 16px+, contraste alto, frases cortas. Base científica del "copy simple convierte el doble".
- **Von Restorff**: lo distinto atrae → el CTA usa un color que NO existe en el resto de la página. Si todo grita, nada destaca.
- **Peak-end**: se recuerda el pico y el final → invertir en el hero (el pico) Y en la thank-you page (el final más descuidado del web).
- **Zeigarnik / goal gradient / endowed progress**: progreso visible motiva; el head start regalado casi duplica el canje (19%→34%, estudio del autolavado) — barra de checkout que arranca en "paso 1 ✓".
- **IKEA**: valoramos lo que co-construimos → configuradores, quiz de plan. Límite: si el esfuerzo supera la motivación, mata antes de nacer.

## Psicología del precio

- **Charm (.99 / 9)**: los precios en 9 venden ~24% más en promedio (Poundstone); Anderson/Simester: $39 vendió MÁS que $34 (el 9 señala "oferta"). → Para VOLUMEN.
- **Redondo por prestigio** (Wadhwa & Zhang, JCR 2015): en compras emocionales/hedónicas el precio redondo "se siente bien"; el .99 contamina la percepción de calidad. → **Regla Culto: premium/servicio de autor = $45,000 redondo; oferta de volumen = $449.**
- **3 tiers**: caro ancla → medio es el objetivo ("Más popular", destacado Von Restorff) → barato es piso. Orden caro→barato refuerza.
- **Reencuadre temporal**: "$33/día" compara contra un café, no contra compras grandes (pennies-a-day, Gourville). Falla si el número diario sigue alto.
- **Sin símbolo $**: menús sin "$" generaron +8.15% de gasto (Cornell) → pricing premium: "1,200" grande + "MXN/mes" chico y gris.
- **Pain of paying**: tarjeta hace pujar 83-100% más que efectivo (Prelec/Simester) → checkout de 1 clic, wallets. Ética: el total con TODO visible antes del último paso (costos sorpresa = causa #1 de abandono, ~48% Baymard).

| | Volumen | Lujo/premium |
|---|---|---|
| Terminación | .99/9 | Redondo |
| Precio | Grande, junto al CTA | Discreto, tras construir deseo |
| Tachados/% desc | Sí | Jamás (daña la marca) |

## Confianza y decisión

- **Stanford Web Credibility (4,500+ personas): 46.1% juzga la credibilidad por el DISEÑO VISUAL** — más que por el contenido. El juicio estético se forma en ~50ms y el halo lo vuelve difícil de revertir. **El hero decide** (por eso existe `hero-handoff.md`).
- Señales Stanford: organización real detrás (dirección, teléfono — crítico en MX), gente real, actualización reciente (blog muerto = empresa muerta), CERO errores (un typo daña más que casi cualquier cosa), sin ads agresivos.
- **Pirámide de compromiso (NN/g)**: confianza para leer → para dar email → para pagar. El popup a los 2 segundos viola la secuencia.
- **Lectura**: el F-pattern es SÍNTOMA de muro de texto, no ideal — diseñar para "layer-cake" (headings escaneables, front-load de las 2 primeras palabras). Z-pattern para heros esparsos.
- **Mirada**: la atención sigue las caras y su dirección (heurística fuerte de eyetracking: la persona de la foto mirando AL formulario).
- **Picture superiority**: recall a 72h ~10% texto vs ~65% con imagen → mostrar el resultado, no describirlo. Pero solo imágenes INFORMATIVAS (el stock decorativo se ignora).

## El mito del color que convierte

"El rojo convierte +21%" (HubSpot 2011) = página verde donde el botón verde se camuflaba. **No existe el color ganador; existe contraste + aislamiento + jerarquía.** Asociaciones de color son culturales y de categoría, no botánicas. Forma: bordes redondeados procesan con menos fricción (Bar & Neta 2006), affordance de botón real, táctil ≥44px.

## Velocidad percibida y micro-feedback

- Límites de Nielsen: 0.1s instantáneo · 1s mantiene flujo · 10s se van. Entre 1-10s: feedback OBLIGATORIO.
- **Skeleton > spinner** (Mejtoft, ECCE 2018): el skeleton dice "ya casi", el spinner dice "espera". Skeletons <10s; barra con estimación >10s. (Código: `01/contadores-skeletons-transiciones.md`.)
- Primero rápido de verdad, luego sentirse rápido (+0.1s real ≈ +8-10% conversión).
- Micro-feedback anti-fricción: validación inline al blur, estados hover/active/loading (botón que no reacciona = doble clic + ansiedad), transiciones 200-300ms que preservan contexto.
- Peak-end en fricción: blindar el ÚLTIMO paso (costo sorpresa o error confuso al final arruina el recuerdo de toda la experiencia).

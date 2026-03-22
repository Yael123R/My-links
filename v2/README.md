# MyLinks - v2 (vibe coding)

## Input - Prompt utilizado

```
Crea una aplicación web con vanilla javascript y vanilla css,
el objetivo es tener un hub de mis redes sociales, con una foto
de perfil, una bio de 1 párrafo y 4 botones apuntando los links
de mis redes sociales. El layout debe ser centrado y con soporte
responsivo. El fondo debe ser una gradiente de colores neon.
Mis datos:
- Nombre: Yael Huamani
- Bio: Desarrollador web en formación. Entusiasta de la tecnología.
- GitHub: https://github.com/Yael123R
- LinkedIn: https://www.linkedin.com/in/yael-yusef-217426312/
- Email: yaelyusefhm123@gmail.com
- Extra: https://www.facebook.com/yaelyusef.huamanimucha.5
Antes de generar el artefacto, dame un resumen del plan que
seguirás para poder pulirlo si es necesario. En este plan debes
resumir cada detalle visual y lógico que has entendido y al final
dame ideas sobre qué aspectos podría personalizar.
```

> El modelo generó un plan previo (con la estructura, estilos, lógica y sugerencias de personalización)
> antes de producir el artefacto, a petición del usuario.

---

## Output

### Stack generado

| Capa       | Tecnología               |
|------------|--------------------------|
| Markup     | HTML5 semántico           |
| Estilos    | Vanilla CSS (inline en `<style>`) |
| Lógica     | Vanilla JavaScript (inline en `<script>`) |
| Fuentes    | Google Fonts (CDN)        |
| Íconos     | SVG inline                |
| Archivo    | Single file `index.html`  |

---

### Estilos

**¿Librería o CSS puro?**
CSS puro, sin frameworks ni librerías externas. Todos los estilos están declarados dentro de una etiqueta `<style>` en el mismo archivo HTML. Se hace uso extensivo de **CSS custom properties** (variables) para mantener coherencia en la paleta de colores y valores de radio.

**Layout**
- `body` con `display: flex`, `align-items: center` y `justify-content: center` para centrado horizontal y vertical absoluto.
- La tarjeta central tiene `max-width: 420px` y `width: 100%` para adaptarse a cualquier viewport.
- Los botones usan `display: flex` internamente para alinear ícono, etiqueta y flecha.
- Media query en `max-width: 480px` para ajustes en mobile (padding reducido, avatar más pequeño, fuente menor).

**Colores**
```css
--neon-pink:   #ff2d78
--neon-cyan:   #00f5ff
--neon-purple: #bf00ff
--neon-lime:   #39ff14
--neon-blue:   #0062ff
--card-bg:     rgba(8, 4, 20, 0.55)   /* glassmorphism oscuro */
```
Fondo base: `#06010f` (negro azulado profundo).
Cada botón tiene el color de marca de su red social aplicado al icono con `linear-gradient`.

**Animaciones**
- `@keyframes drift` — movimiento flotante de los 4 orbs de fondo (traslación + escala), cada uno con duración y dirección distintas (18s, 22s, 28s, 24s).
- `@keyframes cardIn` — entrada de la tarjeta y sus elementos internos con `translateY` + `scale` + `opacity`, usando `cubic-bezier(0.22,1,0.36,1)` (spring suavizado). Cada elemento tiene un `animation-delay` escalonado (0s → 0.65s).
- `@keyframes avatarPulse` — pulso de brillo infinito en el avatar alternando entre cian y morado cada 4s.
- `@keyframes btnClick` — micro-animación de escala al hacer clic en un botón (disparada por JS).
- Transiciones CSS en hover: `transform`, `box-shadow`, `background`, `border-color` (0.18s ease).

---

### Recursos

**Íconos**
SVG inline, paths trazados a mano a partir de los logos oficiales de cada plataforma:
- GitHub — logo del Octocat estilizado
- LinkedIn — logo oficial `in`
- Email — sobre de correo simplificado
- Facebook — logo `f` oficial

No se usa ninguna librería de íconos (sin Font Awesome, sin Heroicons, etc.).

**Emojis**
Ninguno usado en el artefacto final.

**Imágenes**
Ninguna imagen externa. El avatar es un `<div>` con las iniciales `YH` generadas tipográficamente con la fuente Syne 800, sobre un `linear-gradient` morado → cian.

---

### Lógica

JavaScript mínimo (~10 líneas). Su única función es agregar feedback visual al hacer clic en cualquier botón:

```javascript
document.querySelectorAll('.btn').forEach(btn => {
  btn.addEventListener('click', function () {
    this.classList.remove('pulse-click');
    void this.offsetWidth; // fuerza reflow para reiniciar la animación
    this.classList.add('pulse-click');
    setTimeout(() => this.classList.remove('pulse-click'), 380);
  });
});
```

El truco `void this.offsetWidth` obliga al navegador a recalcular el layout, permitiendo que la animación CSS se reinicie incluso si el botón ya tiene la clase aplicada.

---

## Tokens

> Estimación basada en la fórmula `caracteres / 4 ≈ tokens`.

| Concepto                        | Caracteres | Tokens aprox. |
|---------------------------------|------------|---------------|
| Input (prompt del usuario)      | ~780       | ~195          |
| Output 1 — Plan previo (chat)   | ~2.100     | ~525          |
| Output 2 — Artefacto HTML       | ~14.549    | ~3.637        |
| Output 3 — Respuesta post-artefacto (chat) | ~680 | ~170   |
| **Total**                       | **~18.109**| **~4.527**    |

---

## Mejoras detectadas vs v1

1. **Fondo animado con profundidad real** — v1 usaba un gradiente estático en el `body`. v2 implementa 4 orbs independientes con `position: fixed`, `filter: blur()` y animación `drift` continua, creando la ilusión de luz ambiental dinámica que se mueve detrás de la tarjeta.

2. **Efecto glassmorphism en la tarjeta** — v1 probablemente tenía un fondo sólido o semi-opaco simple. v2 usa `backdrop-filter: blur(28px) saturate(1.4)` con borde translúcido y sombra interior (`inset`), dando una sensación de vidrio esmerilado flotando sobre el fondo.

3. **Sistema tipográfico personalizado** — v1 usaba fuentes del sistema o genéricas. v2 importa el par `Syne 800` (display, para el nombre) + `DM Sans 300/500` (body, para bio y botones) desde Google Fonts, creando jerarquía visual deliberada.

4. **Animaciones de entrada escalonadas (stagger)** — v1 no tenía animaciones de carga. v2 aplica `cardIn` con `animation-delay` incremental a cada elemento (avatar → nombre → handle → bio → divisor → botón 1 → botón 2 → botón 3 → botón 4 → footer), generando una secuencia de aparición fluida y orgánica.

5. **Íconos SVG inline por red social** — v1 muy probablemente usaba texto plano o emojis para identificar las redes. v2 incluye SVG paths de los logos oficiales (GitHub, LinkedIn, Email, Facebook) dentro de cápsulas con color de marca, haciendo cada botón reconocible de inmediato sin leer el texto.

6. **Hover contextual por red social** — v1 aplicaba un hover genérico (si es que tenía alguno). v2 define reglas de hover específicas para cada botón: GitHub brilla blanco, LinkedIn en azul `#0077b5`, Email en rosa neon `#ff2d78`, Facebook en azul `#1877f2`, reforzando la identidad de marca en la interacción.

7. **Micro-feedback de clic con JS** — v1 no tenía respuesta táctil al hacer clic. v2 agrega una animación `pulse-click` disparada por JavaScript que escala el botón brevemente (`scale(0.96)`), aportando una sensación de pulsación física y confirmando la acción al usuario.
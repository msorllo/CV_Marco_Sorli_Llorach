# Skill: Efecto Máquina de Escribir (Typewriter Effect)

Esta skill documenta cómo programar e implementar una sección de texto animado que se escribe y borra automáticamente (simulando escritura humana), incluyendo un cursor interactivo. 

Utiliza esta guía para recrear la sección "Lo que nos define" con diferentes textos según lo solicite el usuario.

## 1. Estructura HTML

Crea un contenedor donde se inyectará el texto. Es indispensable asignarle un ID único (ej. `typewriterText`) para que el JavaScript pueda apuntar a él.

```html
<!-- Sección que define el contenedor principal -->
<section class="statements-section">
  <div class="container">
    <div class="statements-content text-center">
      <!-- Título estático superior -->
      <span class="hud-label">Lo que nos define</span>
      
      <!-- Contenedor dinámico del efecto Typewriter -->
      <h2 id="typewriterText" class="typewriter-container"></h2>
    </div>
  </div>
</section>
```

## 2. Estilos CSS (Animación del Cursor)

El efecto requiere un cursor que parpadee constantemente. Añade este CSS a tu archivo de estilos para darle el toque tecnológico/terminal.

```css
/* Estilos para el texto principal (ajustar tipografía y tamaño según diseño) */
.typewriter-container {
  font-family: var(--font-headline, sans-serif);
  font-size: clamp(1.5rem, 4vw, 2.5rem);
  font-weight: 700;
  color: #ffffff;
  min-height: 4rem; /* Evita saltos de altura en el DOM cuando el texto se borra */
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Estilos para el cursor animado */
.typewriter-cursor {
  display: inline-block;
  width: 3px;
  height: 1.1em;
  background-color: var(--accent-blue, #00d4ff);
  margin-left: 6px;
  animation: blinkCursor 0.8s infinite;
  vertical-align: middle;
}

/* Animación de parpadeo infinito */
@keyframes blinkCursor {
  0%, 100% { opacity: 1; }
  50% { opacity: 0; }
}
```

## 3. Lógica JavaScript (El Motor)

Este script contiene las frases y maneja el timing de escritura (ida), pausa y borrado (vuelta). Simula tiempos humanos introduciendo una ligera aleatoriedad entre cada letra.

```javascript
function initTypewriter(elementId, customPhrases) {
  const container = document.getElementById(elementId);
  if (!container) return;

  // Frases dinámicas (se pueden pasar como argumento o usar estas por defecto)
  const phrases = customPhrases || [
    'Digitalizamos negocios para que tú te centres en crecer.',
    'Automatizamos lo repetitivo. Tú decides lo estratégico.',
    'Tu idea + nuestra tecnología = crecimiento imparable.',
    'Precios altamente competitivos. Resultados de élite.',
    'La IA trabaja para ti, no al revés.'
  ];

  let phraseIndex = 0;
  let charIndex = 0;
  let isDeleting = false;
  let cursorEl = container.querySelector('.typewriter-cursor');

  function tick() {
    const currentPhrase = phrases[phraseIndex];
    // Extraer la subcadena actual a mostrar
    const displayed = currentPhrase.substring(0, charIndex);

    // Limpiar el cursor anterior si existiera en el HTML
    if (cursorEl) cursorEl.remove();

    // Actualizar el DOM agregando comillas alrededor de la frase
    const textNode = document.createTextNode('"' + displayed + '"');
    container.innerHTML = '';
    container.appendChild(textNode);
    
    // Generar el cursor si no existe y agregarlo
    if (!cursorEl) {
      cursorEl = document.createElement('span');
      cursorEl.className = 'typewriter-cursor';
    }
    container.appendChild(cursorEl);

    // Lógica de avance / borrado
    if (!isDeleting) {
      // Estado: Escribiendo
      if (charIndex < currentPhrase.length) {
        charIndex++;
        // Velocidad de escritura con leve aleatoriedad
        setTimeout(tick, 30 + Math.random() * 20);
      } else {
        // Pausa al terminar de escribir la frase
        setTimeout(() => { isDeleting = true; tick(); }, 1800);
      }
    } else {
      // Estado: Borrando
      if (charIndex > 0) {
        charIndex--;
        // Velocidad de borrado (más rápida)
        setTimeout(tick, 10);
      } else {
        // Pausa al terminar de borrar y salto a la siguiente frase
        isDeleting = false;
        phraseIndex = (phraseIndex + 1) % phrases.length;
        setTimeout(tick, 300);
      }
    }
  }

  // Iniciar el loop de animación
  tick();
}

// Ejemplo de cómo inicializarlo en el script principal:
// document.addEventListener('DOMContentLoaded', () => {
//   initTypewriter('typewriterText', ['Nuevo texto uno', 'Nuevo texto dos']);
// });
```

## Instrucciones de uso para el Agente

Cuando el usuario pida implementar un bloque de texto dinámico "que se escribe solo":
1. Pregúntale o asimila del prompt las **frases exactas** que quiere mostrar.
2. Crea la estructura HTML (paso 1) en la vista solicitada.
3. Asegúrate de que el CSS (paso 2) esté integrado en la hoja de estilos global o del componente.
4. Llama a `initTypewriter` pasándole el contenedor correcto y el `array` de strings del usuario.

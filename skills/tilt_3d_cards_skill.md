# Skill: Tarjetas 3D con Movimiento e Iluminación (3D Tilt Cards)

Esta skill documenta cómo implementar el sistema de tarjetas 3D interactivas que se utiliza en la sección "Nuestros Servicios". Estas tarjetas reaccionan al movimiento del ratón inclinándose en un espacio 3D, proyectando una sombra dinámica (efecto neón) y generando un brillo (spotlight) que sigue al cursor.

Utiliza esta guía para instruir a cualquier agente a la hora de crear cuadrículas de tarjetas o elementos destacados con un aspecto altamente tecnológico.

## 1. Estructura HTML

La estructura requiere dos contenedores principales: un contenedor exterior que sirve como "detector" del ratón y un contenedor interior que es el que físicamente rota en el espacio 3D.

```html
<!-- El contenedor exterior captura los eventos del ratón -->
<div class="tilt-card">
  
  <!-- El contenedor interior es el que rota y tiene el fondo/borde -->
  <div class="tilt-card__inner">
    
    <!-- Contenido de la tarjeta (textos, iconos, listas, etc.) -->
    <div class="tilt-card__content">
      <div class="icon-wrapper">
        <span class="material-symbols-outlined">rocket_launch</span>
      </div>
      <h3>TÍTULO DEL SERVICIO</h3>
      <p>Descripción detallada de la funcionalidad o servicio a mostrar.</p>
      
      <ul>
        <li>Punto clave 1</li>
        <li>Punto clave 2</li>
      </ul>
    </div>
    
  </div>
</div>
```

## 2. Estilos CSS (Perspectiva y Spotlight)

El CSS es crucial para darle profundidad (perspectiva 3D) y para crear el efecto de "linterna" que ilumina el interior de la tarjeta siguiendo las coordenadas del ratón proporcionadas por el JavaScript.

```css
/* Contenedor Exterior: Establece la cámara 3D */
.tilt-card {
  position: relative;
  perspective: 1200px;
  width: 100%;
  cursor: crosshair; /* O pointer, según el diseño */
}

/* Contenedor Interior: Superficie de cristal/neón */
.tilt-card__inner {
  position: relative;
  width: 100%;
  height: 100%;
  background: rgba(10, 11, 14, 0.9); /* Fondo oscuro base */
  border: 1px solid rgba(0, 212, 255, 0.2); /* Borde sutil inicial */
  border-radius: 16px;
  padding: 2rem;
  transform-style: preserve-3d;
  
  /* Transición de retorno suave cuando el ratón sale */
  transition: transform 0.6s cubic-bezier(0.19, 1, 0.22, 1), 
              box-shadow 0.6s ease, 
              border-color 0.6s ease;
  overflow: hidden; /* Evita que el brillo se salga de los bordes redondos */
}

/* Efecto Spotlight dinámico (La "Linterna") */
.tilt-card__inner::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0; bottom: 0;
  /* Usa variables CSS inyectadas por JS para mover el centro del gradiente */
  background: radial-gradient(
    circle at var(--mouse-x, 50%) var(--mouse-y, 50%), 
    rgba(0, 212, 255, 0.15) 0%, 
    transparent 60%
  );
  opacity: 0;
  transition: opacity 0.3s ease;
  pointer-events: none;
  z-index: 0;
}

/* Activar la linterna al hacer hover */
.tilt-card__inner.is-hovered::before {
  opacity: 1;
}

/* El contenido debe estar por encima de la luz y puede "flotar" hacia el usuario */
.tilt-card__content {
  position: relative;
  z-index: 1;
  /* Opcional: transform: translateZ(30px); para que el texto resalte aún más en 3D */
}
```

## 3. Lógica JavaScript (El Motor 3D y de Luz)

Este script calcula la posición relativa del ratón dentro de la tarjeta y aplica transformaciones matemáticas para rotar el panel interior en los ejes X e Y, simulando una respuesta física al cursor. Además, envía las coordenadas al CSS para el foco de luz.

```javascript
function initTiltCards() {
  const cards = document.querySelectorAll('.tilt-card');
  
  cards.forEach((card) => {
    const inner = card.querySelector('.tilt-card__inner');
    if (!inner) return;

    // EVENTO: Ratón entra en la tarjeta
    card.addEventListener('mouseenter', () => {
      inner.classList.add('is-hovered');
    });

    // EVENTO: Ratón se mueve dentro de la tarjeta
    card.addEventListener('mousemove', (e) => {
      const rect = card.getBoundingClientRect();
      
      // Coordenadas del ratón relativas a la tarjeta
      const mouseRelX = e.clientX - rect.left;
      const mouseRelY = e.clientY - rect.top;
      
      // Normalización: de -0.5 a 0.5 (el centro de la tarjeta es 0)
      const x = (mouseRelX / rect.width) - 0.5;
      const y = (mouseRelY / rect.height) - 0.5;

      // Intensidad de la rotación (Grados)
      const tiltX = 25; 
      const tiltY = 20;
      const depth = 25; // Traslación Z (lo saca hacia la pantalla)
      const scale = 1.03; // Ligero zoom al hacer hover

      // Aplicar rotación 3D intensa + profundidad
      inner.style.transform = `perspective(1200px) rotateY(${x * tiltX}deg) rotateX(${y * -tiltY}deg) translateZ(${depth}px) scale(${scale})`;
      
      // La transición se hace lineal/rápida mientras nos movemos para que sea instantáneo
      inner.style.transition = 'transform 0.08s linear, box-shadow 0.08s linear, border-color 0.2s ease';

      // Sombra dinámica de Neón (Se proyecta en el lado contrario al ratón)
      const shadowX = -x * 30;
      const shadowY = y * 30;
      
      // Aplicar sombra base y aura brillante
      inner.style.boxShadow = `${shadowX}px ${shadowY}px 50px rgba(0,0,0,0.5), 0 0 50px rgba(0,212,255,0.12), 0 0 100px rgba(0,212,255,0.05)`;
      
      // Cambiar dinámicamente el color del borde dependiendo del eje X (Efecto cromático)
      const hue = Math.round(190 + x * 30);
      inner.style.borderColor = `hsla(${hue}, 100%, 55%, 0.5)`;

      // Inyectar coordenadas % en CSS para la luz interior (Spotlight)
      card.style.setProperty('--mouse-x', `${(mouseRelX / rect.width) * 100}%`);
      card.style.setProperty('--mouse-y', `${(mouseRelY / rect.height) * 100}%`);
    });

    // EVENTO: Ratón sale de la tarjeta
    card.addEventListener('mouseleave', () => {
      inner.classList.remove('is-hovered');
      
      // Limpiar estilos en línea
      inner.style.transform = '';
      inner.style.boxShadow = '';
      inner.style.borderColor = '';
      
      // Restaurar transición lenta para que vuelva a su sitio suavemente
      inner.style.transition = 'transform 0.6s cubic-bezier(0.19,1,0.22,1), box-shadow 0.6s ease, border-color 0.6s ease';
    });
  });
}

// Ejemplo de inicialización:
// document.addEventListener('DOMContentLoaded', initTiltCards);
```

## Instrucciones de uso para el Agente

Cuando el usuario te pida crear "tarjetas animadas en 3D que sigan el ratón" similares a las de "Nuestros Servicios":
1. Utiliza la estructura HTML de doble capa: `.tilt-card` envolviendo a `.tilt-card__inner`.
2. Añade dentro el contenido del usuario en un envoltorio adicional opcional `.tilt-card__content`.
3. Aplica los estilos CSS provistos asegurándote de usar el pseudo-elemento `::before` para la iluminación ligada a las variables `--mouse-x` y `--mouse-y`.
4. Ejecuta la función JavaScript `initTiltCards()` para enlazar los eventos del DOM a las matemáticas de rotación.
5. Puedes ajustar el color base (reemplazando los RGB correspondientes al azul eléctrico/cyan por el color corporativo que te pidan).

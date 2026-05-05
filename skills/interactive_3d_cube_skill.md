---
name: interactive-3d-shape
description: Instrucciones paso a paso para crear un elemento 3D (como un cubo) que rota interactivamente siguiendo el cursor del ratón, con una animación idle por defecto.
---

# Skill: Elemento 3D Interactivo (Cubo) que Sigue al Ratón

## Objetivo
El propósito de esta skill es enseñar a un agente cómo construir la sección de "Laboratorio Interactivo" o "Ingeniería de Precisión" de MISTEREIA, donde un elemento 3D (un cubo) rota siguiendo la posición exacta del cursor del ratón cuando el usuario pasa por encima, y vuelve a una animación giratoria infinita ("idle") cuando el ratón sale.

Esta técnica maximiza el "Vibe" visual interactivo utilizando matemáticas simples de JavaScript y transformaciones CSS 3D nativas, manteniendo un alto rendimiento sin necesidad de librerías como Three.js.

## 1. Estructura HTML (El DOM)
Necesitamos un contenedor principal que aporte la **perspectiva** y un contenedor interno que mantenga el **espacio 3D**. Dentro, se colocan las caras de la forma geométrica (en este caso, 6 caras de un cubo).

```html
<!-- Contenedor principal con la perspectiva -->
<div class="mistereia-core">
  <!-- Contenedor que mantiene el entorno 3D -->
  <div class="cube-3d">
    <!-- Caras del cubo que tambien podrian ser otros objetos 3D  (pueden contener texto, iconos, imágenes) -->
    <div class="front">🚀</div>
    <div class="back">IA</div>
    <div class="right">⚙️</div>
    <div class="left">💎</div>
    <div class="top">⚡</div>
    <div class="bottom">🔥</div>
  </div>
</div>
```

**Nota para el Agente:** El contenido de las caras (`<div class="front">`, etc.) puede ser cualquier cosa que pida el usuario: textos, iconos de Material Symbols, o imágenes. 

## 2. Estilos CSS (El Motor 3D)
El truco de CSS es usar `perspective` en el contenedor padre, y `transform-style: preserve-3d` en el hijo. Luego, cada cara se traslada en el eje Z (`translateZ`) a la mitad del tamaño total del cubo, y se rota según corresponda.

```css
/* 1. Contenedor Padre: Define la profundidad (perspective) */
.mistereia-core {
  width: 140px; /* Tamaño del cubo */
  height: 140px;
  margin: 2rem auto 3rem;
  perspective: 800px; /* Cuanto menor es, más extrema es la deformación 3D */
}

/* 2. Contenedor del Cubo: Mantiene el 3D y la animación por defecto */
.cube-3d {
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d; /* CRÍTICO para que las caras no queden planas */
  animation: rotateCube 20s infinite linear; /* Rotación automática (Idle) */
}

/* 3. Estilos base para todas las caras */
.cube-3d div {
  position: absolute;
  width: 140px;
  height: 140px;
  background: rgba(0, 212, 255, 0.05); /* Glass sutil */
  border: 2px solid var(--accent-blue);
  box-shadow: 0 0 20px rgba(0, 212, 255, 0.2); /* Glow */
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
  backface-visibility: visible;
}

/* 4. Posicionamiento de las caras (70px es la mitad de 140px) */
.cube-3d .front  { transform: translateZ(70px); }
.cube-3d .back   { transform: rotateY(180deg) translateZ(70px); border-color: var(--accent-yellow); box-shadow: 0 0 20px rgba(255, 215, 0, 0.2); }
.cube-3d .right  { transform: rotateY(90deg) translateZ(70px); }
.cube-3d .left   { transform: rotateY(-90deg) translateZ(70px); border-color: var(--accent-yellow); box-shadow: 0 0 20px rgba(255, 215, 0, 0.2); }
.cube-3d .top    { transform: rotateX(90deg) translateZ(70px); }
.cube-3d .bottom { transform: rotateX(-90deg) translateZ(70px); }

/* 5. Animación Idle (Cuando no hay ratón) */
@keyframes rotateCube {
  from { transform: rotateX(0) rotateY(0); }
  to   { transform: rotateX(360deg) rotateY(360deg); }
}
```

## 3. JavaScript (La Magia Magnética)
El script calcula la posición del ratón de `-0.5` a `0.5` basándose en las dimensiones del contenedor `.mistereia-core`, y aplica una rotación dinámica al `.cube-3d`.

```javascript
function initInteractiveCube() {
  const cube = document.querySelector('.cube-3d');
  const container = document.querySelector('.mistereia-core');

  if (container && cube) {
    // Cuando el ratón se mueve DENTRO del contenedor
    container.addEventListener('mousemove', (e) => {
      // 1. Obtener coordenadas y dimensiones relativas a la pantalla
      const { left, top, width, height } = container.getBoundingClientRect();
      
      // 2. Normalizar posición X e Y entre -0.5 y 0.5
      const x = (e.clientX - left) / width - 0.5;
      const y = (e.clientY - top) / height - 0.5;
      
      // 3. Aplicar rotación. Multiplicar por 100 define la intensidad/sensibilidad (grados)
      // Nota: Y mueve el eje X, X mueve el eje Y.
      cube.style.transform = \`rotateX(\${y * 100}deg) rotateY(\${x * 100}deg)\`;
      
      // 4. Detener la animación automática (idle) mientras el usuario interactúa
      cube.style.animation = 'none';
    });

    // Cuando el ratón SALE del contenedor
    container.addEventListener('mouseleave', () => {
      // 1. Limpiar transform manual
      cube.style.transform = '';
      // 2. Reactivar la animación automática
      cube.style.animation = 'rotateCube 20s infinite linear';
    });
  }
}

// Llamar a la función cuando el DOM cargue
document.addEventListener('DOMContentLoaded', initInteractiveCube);
```

## Instrucciones para el Agente
Cuando el usuario te pida "crear un apartado con un elemento 3D que siga al ratón" o referencie esta skill:
1. Pide al usuario qué **textos, iconos o colores** desea en las 6 caras del cubo.
2. Genera el código HTML y envuélvelo en una nueva sección con un diseño coherente a MISTEREIA.
3. Asegúrate de añadir el CSS exacto manteniendo el `transform-style: preserve-3d` y las variables CSS de MISTEREIA.
4. Inyecta la función JS, adaptando los selectores (`document.querySelector`) si has renombrado las clases para evitar conflictos si hay múltiples cubos en la misma página.

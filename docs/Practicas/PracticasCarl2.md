## Práctica 2: Pruebas de Calibración e Impresión del Mecanismo Articulado
Fecha: 25 de septiembre de 2026

### Introducción y Objetivo
En la segunda sesión pasamos del modelo digital a la fabricación física mediante la tecnología FDM. El objetivo fue realizar pruebas de calibración de flujo, tolerancia e impresión del mecanismo articulado proyectado en la Práctica 1.

### Preparación del Archivo en el Laminador (Slicer)
Se importó el archivo STL a Bambu Studio / Ultimaker Cura configurando los siguientes parámetros de proceso para PLA:
- Altura de capa: $0.2\text{ mm}$.
- Relleno (Infill): $20\%$ con patrón Giroide para resistencia estructural multidireccional.
- Soportes: Desactivados (se validó que los ángulos de voladizo fueran inferiores a $45^\circ$).
- Adherencia a la placa: Falda (Skirt) simple de 2 líneas.

### Pruebas de Tolerancia Muestra (Tolerance Test)
Antes de lanzar el mecanismo completo, se imprimió un espécimen de prueba con varias tolerancias horizontales ($0.1\text{ mm}$, $0.2\text{ mm}$, $0.3\text{ mm}$, $0.4\text{ mm}$, $0.5\text{ mm}$).
Figura 2: Prueba de tolerancias post-impresión para validación de holgura de movimiento.

Resultados del test:
- Las cavidades de $0.1\text{ mm}$ y $0.2\text{ mm}$ quedaron totalmente fusionadas (welded).
- La holgura de $0.3\text{ mm}$ requirió un esfuerzo mecánico moderado para liberarse.
- La holgura de $0.4\text{ mm}$ giró de manera fluida inmediatamente al retirar la pieza de la cama.

### Impresión Final y Verificación
Con base en la prueba, se confirmó la tolerancia de $0.4\text{ mm}$ configurada en SolidWorks y se procedió a imprimir el mecanismo concéntrico completo.
- Tiempo de impresión: $42\text{ minutos}$.
- Consumo de material: $14.5\text{ g}$.
- Evaluación final: La pieza articulada se liberó con un suave giro manual, logrando una rotación limpia de $360^\circ$ sin trabas.
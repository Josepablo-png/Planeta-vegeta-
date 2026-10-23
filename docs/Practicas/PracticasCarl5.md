## Práctica 5: Ensamblaje Avanzado y Análisis de Interferencias
Fecha: 16 de octubre de 2026

### Objetivo
Crear un ensamblaje complejo que integre las piezas previamente diseñadas e inspeccionar posibles colisiones, rangos de movimiento y holguras operativas para asegurar la viabilidad de fabricación.

### Metodología de Ensamblaje
1. Inserción del Componente Base: Fijación de la carcasa o chasis estructural en el origen de coordenadas de SolidWorks.
2. Asignación de Relaciones de Posición (Mates):
  - Coincidentes y Concéntricas: Para la alineación de pernos y cavidades.
  - Avanzadas (Ancho y Límite de Ángulo): Para restringir el rango de rotación de los componentes móviles entre $0^\circ$ y $120^\circ$.
  - Mecánicas (Engranajes / Leva): Para transmitir la relación de transmisión solicitada en el proyecto.

Figura 5: Vista general del ensamblaje articulado final con componentes integrados.

### Estudio de Interferencias y Detección de Colisiones
Se ejecutó la herramienta Detección de interferencias en SolidWorks a lo largo de todo el ciclo de movimiento:
[Análisis de Interferencias]

Resultados:
- Interferencias detectadas: 1 (Menor)
- Ubicación: Buje de articulación secundaria
- Volumen de interferencia: $0.03\text{ mm}^3$
- Solución aplicada: Corrección de cota de diámetro exterior de perno de $5.0\text{ mm}$ a $4.85\text{ mm}$.

### Conclusiones Globales del Compendio
A través de la realización de estas cinco prácticas, se cubrió exitosamente el ciclo completo de diseño de producto en ingeniería: desde la concepción en modelado digital (Print-in-Place), su validación y prueba mediante manufactura aditiva (FDM), la digitalización de piezas físicas mediante escaneo e ingeniería inversa, hasta la correcta integración y verificación en ensamblajes complejos dentro de SolidWorks.
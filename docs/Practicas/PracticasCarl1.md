## Práctica 1: Modelado de Mecanismo Concéntrico Articulado en SolidWorks
Fecha: 18 de septiembre de 2026

### Introducción y Objetivo de la Clase
En esta sesión nos enfocamos en aprender los conceptos clave de diseño para prepararnos en la creación de piezas articuladas y con movimiento en SolidWorks. El objetivo principal fue entender cómo estructurar un mecanismo tipo Print-in-Place a nivel digital, logrando que una sola pieza contenga partes móviles independientes diseñadas directamente desde el croquis.

### Guía de Materiales, Ventajas y Desventajas
Durante la clase revisamos los filamentos de uso frecuente en impresión 3D FDM, comprendiendo sus propiedades térmicas, ventajas y desventajas al momento de seleccionar el material adecuado para un proyecto:

| Material | Temp. Boquilla | Temp. Cama | Ventajas Principales | Desventajas / Consideraciones |
| :--- | :--- | :--- | :--- | :--- |
| PLA (Ácido Poliláctico) | $190^\circ\text{C} - 220^\circ\text{C}$ | $45^\circ\text{C} - 60^\circ\text{C}$ | Biodegradable, muy fácil de imprimir, buena rigidez. | Baja resistencia térmica y mecánica (se deforma a $> 50^\circ\text{C}$). |
| PETG | $230^\circ\text{C} - 250^\circ\text{C}$ | $70^\circ\text{C} - 80^\circ\text{C}$ | Resistente al impacto y química, impermeable, fácil impresión. | Propenso a hilado (stringing), absorbe humedad. |
| ABS | $230^\circ\text{C} - 260^\circ\text{C}$ | $90^\circ\text{C} - 110^\circ\text{C}$ | Alta resistencia térmica y mecánica, fácil de pulir. | Requiere impresora cerrada, emite vapores nocivos. |
| TPU (Flexible) | $210^\circ\text{C} - 230^\circ\text{C}$ | $30^\circ\text{C} - 60^\circ\text{C}$ | Gran elasticidad, resistente al desgaste y amortiguador. | Difícil de imprimir, velocidad lenta, sensible a la humedad. |
| PC (Policarbonato) | $260^\circ\text{C} - 310^\circ\text{C}$ | $100^\circ\text{C} - 120^\circ\text{C}$ | Extremadamente resistente al impacto y altas temperaturas. | Muy difícil de imprimir, requiere alta temperatura de cama y boquilla. |

### Tolerancias para Diseños Print-in-Place
Aprendimos que la tolerancia (holgura entre paredes móviles) determina el éxito de la articulación sin necesidad de ensamblaje posterior:
- Tolerancia recomendada: $0.3\text{ mm} - 0.4\text{ mm}$ para un libre movimiento.
- Tolerancia ajustada: $0.2\text{ mm}$ (requiere impresoras muy bien calibradas).

### Metodología de Modelado en SolidWorks
#### Croquis Base Concéntrico: Se inició en el plano Planta dibujando circunferencias concéntricas concuerden con los diámetros interno y externo del mecanismo.
#### Definición de Cotas y Holgura:
  - Diámetro interior del núcleo: $D_1 = 20\text{ mm}$.
  - Anillo intermedio con holgura radial $\delta = 0.4\text{ mm}$.
  - Diámetro exterior total: $D_2 = 60\text{ mm}$.
#### Estructura Interna del Eje: Se diseñaron topes cónicos inclinados a $45^\circ$ para evitar la separación vertical de las piezas respetando las capacidades de puentes (bridging) de la impresión FDM.
### Operación Extrusión y Sólidos Multicuerpo: Se utilizó Extruir saliente/base desactivando la opción "Fusionar resultado" para mantener los cuerpos sólidos independientes en un solo archivo.

Figura 1: Vista transversal del modelado concéntrico articulado mostrando las holguras inter-cuerpos.
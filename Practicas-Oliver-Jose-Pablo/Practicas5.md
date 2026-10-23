## Práctica 5: Maquinado CNC y router de Asia Robotics 🤖

En esta clase, el profesor Oliver nos mostró el sistema de simulación donde se observa el recorrido de la fresa sobre una figura predeterminada. Explicó los **códigos G** fundamentales que controlan los movimientos de la máquina para evitar colisiones y daños al equipo.

### Códigos G empleados:
| Código | Función |
| :--- | :--- |
| `G00` | Posicionamiento rápido (sin cortar) |
| `G01` | Interpolarización lineal (corte en línea recta a velocidad programada) |
| `G02` / `G03` | Interpolación circular (sentido horario / antihorario) |

*Realizamos una simulación previa utilizando estos códigos para trazar una figura y verificar las trayectorias.*

---

### Proceso de Trabajo en Router CNC:
Debido al tiempo disponible para diseñar un modelo desde cero, utilizamos una plantilla vectorizada descargada de internet.

1. **Importación:** Se importó el diseño al software **VCarve Pro**.
2. **Configuración:** Se configuró el espesor del material y los parámetros de corte (velocidad de avance, RPM del husillo y profundidad por pasada).
3. **Generación del Código G:** Se calculó la trayectoria de la herramienta y se guardó el archivo `.gcode` / `.tap` en una memoria USB.
4. **Preparación del Router (Asia Robotics):**
   * Limpieza profunda de la base y la cama de sacrificio.
   * Fijación segura del material.
   * Conexión de la memoria USB y carga del archivo en el control.
   * Calibración del origen y ceros de pieza (**WCS** - *Work Coordinate System*).
   * Ejecución del programa bajo supervisión continua.

### Resultado:
Como práctica individual/grupal, fresamos la figura seleccionada: un diseño de **Goku**.

![Fresado de Goku CNC](ruta/a/tu/foto_goku.jpg)
# Práctica 5: Introducción al Maquinado CNC, Simulación en NC Viewer y Router Asia Robótica
**Ubicación:** Fab Lab Puebla  
**Fecha:** 9 de octubre de 2026  

---

## 1. Introducción y el Papel del NC Viewer en el Proceso CAM
* Antes de operar una máquina de control numérico computarizado (CNC) física, es indispensable contar con herramientas de validación digital que nos permitan prevenir errores graves de colisión, rotura de fresas o fallas en el material. Aquí es donde entra NC Viewer (`ncviewer.com`), una plataforma de simulación basada en navegador web que cumple un rol fundamental en la etapa previa al maquinado real.
* El propósito principal de utilizar NC Viewer es visualizar de manera gráfica y tridimensional las trayectorias que seguirá la herramienta de corte a partir del código G generado. Nos sirve como un filtro de seguridad y aprendizaje para:
  * **Validación de cotas:** Comprobar que las coordenadas programadas correspondan exactamente al diseño deseado sin saltos extraños.
  * **Prevención de colisiones:** Identificar si existen movimientos erróneos en los ejes o profundidades de corte excesivas (Z) antes de enviarlas al controlador de la máquina real.
* **Traducción cinemática:** Entender la lógica matemática detrás de los comandos numéricos y cómo el software traduce los vectores en instrucciones físicas para los motores.

---

## 2. Programación y Control en G-Code

### 2.1. Sintaxis y Diccionario Ampliado de Comandos G y M
El código G y el código M son los lenguajes estandarizados utilizados para indicarle al control numérico de la máquina las trayectorias geométricas, las velocidades, el sentido de giro y las funciones auxiliares de operación:

| Comando / Var. | Descripción / Función | Clasificación |
| :--- | :--- | :--- |
| `G0` / `G00` | Movimiento rápido de posicionamiento (sin realizar corte). | Movimiento |
| `G1` / `G01` | Movimiento lineal controlado con avance de corte definido por F. | Interpolación |
| `G2` / `G02` | Interpolación circular en sentido horario (CW - Clockwise). | Interpolación |
| `G3` / `G03` | Interpolación circular en sentido antihorario (CCW). | Interpolación |
| `G04` | Temporización o pausa (Dwell) mediante parámetro P o X. | Control Temporal |
| `G17` | Selecciona el plano de trabajo XY. | Selección de Plano |
| `G18` | Selecciona el plano de trabajo XZ. | Selección de Plano |
| `G19` | Selecciona el plano de trabajo YZ. | Selección de Plano |
| `G20` | Define el sistema de unidades en pulgadas. | Unidades |
| `G21` | Define el sistema de unidades en milímetros. | Unidades |
| `G28` | Retorno automático al punto de referencia de la máquina (Home). | Referencia |
| `G40` | Cancela la compensación de radio de la herramienta. | Compensación |
| `G41` | Activa la compensación de radio a la izquierda del contorno. | Compensación[cite: 5] |
| `G42` | Activa la compensación de radio a la derecha del contorno. | Compensación[cite: 5] |
| `G43` | Activa la compensación de longitud de la herramienta. | Compensación[cite: 5] |
| `G53` | Utiliza el sistema de coordenadas de la máquina (MCS). | Sistema Coord.[cite: 5] |
| `G54` a `G59` | Selecciona Sistemas de Coordenadas de Trabajo (WCS). | Sistema Coord.[cite: 5] |
| `G80` | Cancela los ciclos fijos (perforación, cajeado, roscado). | Ciclos Fijos[cite: 5] |
| `G81` | Ciclo fijo de taladrado simple. | Ciclos Fijos[cite: 5] |
| `G83` | Ciclo fijo de taladrado con descargas profundas (Peck Drilling). | Ciclos Fijos[cite: 5] |
| `G90` | Programación en Coordenadas Absolutas. | Modo Coord.[cite: 5] |
| `G91` | Programación en Coordenadas Incrementales. | Modo Coord.[cite: 5] |
| `G92` | Establece temporalmente el sistema de coordenadas actual. | Modo Coord.[cite: 5] |
| `X, Y, Z` | Coordenadas espaciales de posición en los ejes tridimensionales. | Posición[cite: 5] |
| `I, J, K` | Distancias incrementales al centro de un arco en ejes X, Y, Z. | Parám. Arco[cite: 5] |
| `F` | Velocidad de avance (Feed Rate, p. ej., F180 o F300). | Parám. Corte[cite: 5] |
| `S` | Velocidad del husillo (Spindle Speed en RPM, p. ej., S18000). | Parám. Corte[cite: 5] |
| `T` | Selección de número de herramienta (Tool). | Parám. Herram.[cite: 5] |
| `M3` / `M03` | Encender el husillo en sentido horario (CW). | Función Aux. (M)[cite: 5] |
| `M5` / `M05` | Apagar el husillo (Spindle Stop). | Función Aux. (M)[cite: 5] |
| `M30` | Fin del programa, reinicio de ciclos y rebobinado. | Función Aux. (M)[cite: 5] |

### 2.2. Simulación y Validación en NC Viewer (Ejercicio del Pentagrama)[cite: 5]
Como parte del aprendizaje práctico en clase con el maestro Oliver, pusimos a prueba la lógica de coordenadas y la estructura del código G realizando la simulación de un pentagrama en la plataforma NC Viewer[cite: 5]. Interpretamos y verificamos líneas de código como los posicionamientos rápidos (`G00`), los avances lineales de corte (`G01`) y las interpolaciones circulares (`G02`), observando de qué manera la herramienta ejecuta la geometría en el plano 2D/3D antes de pasar al equipo real[cite: 5]:

```gcode
G21
G90
G00 X50.0 Y0.0 Z5.0 F1000
G01 Z-1.0 F300
G02 X50.0 Y0.0 I-50.0 J0.0 F500
G00 X0.0 Y50.0
G01 X29.39 Y-40.45
G01 X-47.55 Y15.45
G01 X47.55 Y15.45
G01 X-29.39 Y-40.45
G01 X0.0 Y50.0
G00 Z5.0
M02
## Práctica 3: Escaneo 3D y Digitalización de Pieza Física
Fecha: 2 de octubre de 2026

### Objetivo de la Sesión
Introducción a las tecnologías de Digitalización Tridimensional e Ingeniería Inversa. Se utilizó un escáner 3D óptico de luz estructurada para capturar la geometría de una pieza física real y llevarla a un entorno CAD.

### Proceso de Captura y Digitalización
1. Acondicionamiento de la pieza: Se aplicó un spray matificante sobre las partes brillantes o transparentes para evitar reflexiones erróneas en el sensor del escáner.
2. Colocación de Puntos de Referencia (Markers): Se pegaron puntos adhesivos autorreflectantes para facilitar la alineación en tiempo real.
3. Escaneo Óptico: Se realizaron barridos en malla de $360^\circ$ utilizando una mesa giratoria automatizada.

| Parámetro de Escaneo | Valor / Configuración |
| :--- | :--- |
| Resolución de Malla | $0.1\text{ mm}$ |
| Modo de Captura | Textura con Color + Geometría |
| Tiempo de Procesamiento | $15\text{ min}$ |

Figura 3: Nube de puntos y malla poligonal generada tras el proceso de escaneo.

### Limpieza de Malla y Postprocesamiento
A través del software del escáner (Geomagic / MeshLab), se realizaron las siguientes operaciones:
- Eliminación de puntos flotantes (outliers).
- Cierre de agujeros (Hole Filling) automática y manualmente.
- Simplificación de polígonos manteniendo la definición geométrica.
- Exportación final en formato STL/OBJ.
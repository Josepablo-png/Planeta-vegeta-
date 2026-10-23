## Práctica 4: Ingeniería Inversa y Remodelado CAD en SolidWorks
Fecha: 9 de octubre de 2026

### Objetivo
A partir del archivo STL/OBJ obtenido mediante el escaneo 3D en la práctica previa, realizar la ingeniería inversa utilizando SolidWorks para convertir la malla ortogonal en un modelo sólido parametricamente editable.

### Proceso de Reconstrucción Geométrica
1. Importación de la Malla: Se cargó el archivo STL en SolidWorks utilizando el módulo Geometría de Malla.
2. Alineación de Coordenadas: Se definieron los planos primario (Planta), secundario (Alzado) y terciario (Perfil) alineados a la orientación de la malla digitalizada.
3. Extracción de Secciones (Croquis de Malla): Se crearon planos interceptores para generar curvas de corte que delimitaran el perfil real de la pieza física.
4. Modelado Paramétrico:
  - Se aplicaron operaciones de Extrusión, Revolución y Vaciado sobre los perfiles trazados a partir de la malla guía.
  - Se ajustaron los redondos y chaflanes según cotas estándar normalizadas.

Figura 4: Superposición de la malla de escaneo 3D con el modelo paramétrico editable en SolidWorks.

### Análisis de Desviación Geométrica
Se realizó un análisis de comparación dimensional comparando el modelo sólido generado contra la malla escaneada original:
- Desviación Máxima Permitida: $\pm 0.15\text{ mm}$.
- Resultado: El $94.2\%$ de las superficies del modelo parametrizado se mantuvieron dentro del rango de tolerancia objetivo.
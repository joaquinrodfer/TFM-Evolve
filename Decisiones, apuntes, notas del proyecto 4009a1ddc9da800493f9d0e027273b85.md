# Decisiones, apuntes, notas del proyecto

- Apuntad todo lo que tengáis: ideas, propuestas, cambios, etc…

[Fichas proyectos](https://app.notion.com/p/Fichas-proyectos-4019a1ddc9da80d1926ad9db13de8537?pvs=21)

## <u>**Lluvia de ideas iniciales**</u>

 **( se pueden añadir, quitar, propones ssobre  lo que ya hay o quitar) <u>:</u>**

Para darles el abanico más amplio posible, he agrupado este listado en las **6 grandes áreas de Indra** (Defensa, Espacio, Gestión de Tráfico Aéreo, Movilidad/Transportes, Ciberseguridad y Tecnologías Digitales/Minsait) que consumen masivamente geodatos e inteligencia artificial.

Aquí tienen un listado extenso de proyectos de gran escala que encajan directamente con sus líneas de negocio:

-VERDE: Vicente

-MORADO : Rafa

-AZUL: Joaquín

**🛡️ 1. Defensa y Sistemas de Combate (Sistemas de Mando y Control / C4ISR)**

- **Detección automática de anomalías y patrones de camuflaje** en imágenes de satélite o drones mediante visión computacional.
- **Predicción táctica de trayectorias e intenciones de vuelo** para aeronaves o misiles no identificados.
- **Asignación automatizada de contramedidas antiaéreas** en tiempo real mediante aprendizaje por refuerzo ante ataques de enjambres de drones (*Anti-UAV*).
- **Planificador de rutas logísticas militares seguras** optimizado por IA frente a amenazas dinámicas, orografía del terreno y meteorología.
- **Fusión inteligente de sensores (Radar, Lidar, Óptico)** para generar mapas de conciencia situacional unificados en el campo de batalla. + RAFA + Joaquín
- **Análisis predictivo de fallos en sistemas de armamento y vehículos de combate** (*Mantenimiento Predictivo Militar*).
- **Clasificación automática de señales de radar e identificación de emisores** mediante procesamiento de señales con aprendizaje profundo (*Guerra Electrónica*).

**🛰️ 2. Espacio y Observación de la Tierra (Sistemas Satelitales y GeoINT)**

- **Monitorización automatizada de infraestructuras críticas y fronteras** mediante imágenes de radar satelital (SAR) para detectar cambios milimétricos en el terreno.
- **Detección precoz y predicción de la propagación de incendios forestales** combinando imágenes satelitales, gemelos digitales del terreno y modelos meteorológicos.
- **Clasificación automática del uso del suelo y deforestación ilegal** a escala global mediante modelos de segmentación semántica de imágenes satelitales.
- **Detección y seguimiento de vertidos de petróleo y pesca ilegal** en alta mar cruzando datos de radar satelital con sistemas de identificación de barcos (AIS).
- **Predicción de desastres naturales (inundaciones, desprendimientos)** mediante el análisis de series temporales de geodatos e índices de vegetación/humedad + RAFA + Joaquín

**✈️ 3. Gestión de Tráfico Aéreo (ATM - Air Traffic Management)**

- **Predicción de conflictos de trayectorias aéreas a largo plazo** y sugerencia automática de rutas de evasión óptimas para los controladores.
- **Optimización del flujo de tráfico en aeropuertos** mediante modelos predictivos de retrasos, gestión de colas en pistas y condiciones climáticas severas. + Joaquín
- **Enrutamiento eco-eficiente de aeronaves** para minimizar el consumo de combustible y las emisiones de CO₂ basándose en corrientes de viento globales en tiempo real.
- **Gemelo digital de la gestión del espacio aéreo** para simular la integración segura de aerotaxis y drones de reparto en ciudades (*U-Space*).

**🚘 4. Movilidad, Transportes y Puertos Inteligentes**

- **Planificación predictiva de mantenimiento de vías ferroviarias y carreteras** procesando datos geoespaciales analizados por visión artificial en trenes/vehículos de auscultación.
- **Sistema de optimización de rutas de transporte público bajo demanda** basado en predicciones de densidad de población y movilidad urbana en tiempo real. + RAFA
- **Gemelo digital portuario para la gestión y predicción del tráfico de mercancías**, atraque de buques y logística de contenedores en entornos de costa.
- **Plataforma de guiado autónomo e inteligente para flotas de vehículos en entornos cerrados** (aeropuertos, puertos o bases logísticas) utilizando geodatos de alta precisión.

**🔒 5. Ciberdefensa y Ciberinteligencia**

- **Correlación espacial de ciberamenazas** para geolocalizar el origen de ciberataques coordinados contra infraestructuras críticas y predecir el próximo objetivo.
- **Detección de manipulación o spoofing de datos de geolocalización (GPS/GNSS)** en sistemas de navegación mediante modelos de IA anomalía.
- **Procesamiento de lenguaje natural (NLP) geolocalizado** para el análisis de fuentes abiertas (OSINT) y detección temprana de crisis geopolíticas o disturbios.

**📊 6. Consultoría Digital y Smart Cities (Minsait)**

- **Plataforma de optimización de la red eléctrica y detección de pérdidas/fraudes** basada en la ubicación geográfica de los activos y patrones de consumo.
- **Predicción espacial del rendimiento agrícola y optimización de riegos** para el sector agroalimentario combinando IoT, meteorología y geodatos.
- **Planificación urbana inteligente y simulación del impacto ambiental** (islas de calor, contaminación acústica y del aire) mediante el análisis de la morfología de las ciudades en 3D.

**📊 7. Basura-meteorología** ( idea de Rafa , friky del mar, preguntar si interesa y desarrollo la idea). —

*Actualización ideas 22/10/2026:*

**1. Análisis de Accesibilidad y "Desiertos Verdes" (Zonas Verdes Urbanas)**

- **Objetivo:** Evaluar si todos los vecindarios de una ciudad tienen un acceso equitativo a parques y espacios naturales a una distancia caminable (por ejemplo, a menos de 10 o 15 minutos a pie).
- **Datos geoespaciales necesarios:** Capas vectoriales de parques y áreas recreativas (sacadas de OpenStreetMap), red de calles para calcular isócronas (áreas de alcance a pie) y datos censales de población.
- **Aportación del equipo:** Los expertos en geoespacial pueden calcular las redes de caminos y las áreas de influencia (*buffers* / isócronas), mientras el tercer integrante puede encargarse de la parte de análisis socioeconómico o la interfaz visual.
- **Impacto Smart City:** Ayuda a la planificación urbana sostenible y a la salud pública.

**2. Optimización de Rutas de Recogida de Residuos**

- **Objetivo:** Diseñar un modelo de rutas eficientes para camiones de basura o contenedores inteligentes, buscando reducir el consumo de combustible y las emisiones de CO2.
- **Datos geoespaciales necesarios:** Ubicación de contenedores (puntos), red viaria urbana (líneas con restricciones de sentido y giros) y ubicación de la planta de tratamiento.
- **Aportación del equipo:** Análisis de redes (*Network Analysis*) utilizando herramientas como QGIS o librerías de Python (`NetworkX`, `GeoPandas`).
- **Impacto Smart City:** Eficiencia de recursos públicos y reducción de la huella de carbono municipal.

**3. Estudio de Islas de Calor Urbanas (UHI) y su Relación con el Arbolado**

- **Objetivo:** Identificar qué zonas de la ciudad sufren mayores temperaturas durante las olas de calor y analizar si la falta de arbolado o los materiales de construcción son los culpables.
- **Datos geoespaciales necesarios:** Datos ráster de temperatura superficial (vía imágenes de satélite gratuitas como Landsat o Sentinel), censo de arbolado urbano y polígonos de edificios.
- **Aportación del equipo:** Procesamiento de imágenes de satélite, cálculo de índices de vegetación (NDVI) y superposición de capas.
- **Impacto Smart City:** Resiliencia climática y adaptación al cambio climático urbano.

**4. Evaluación de la Conectividad Ciclista y Puntos Críticos**

- **Objetivo:** Analizar la red de carriles bici actual para detectar "roturas" en la conectividad, tramos desconectados o zonas con alta densidad de incidentes viales.
- **Datos geoespaciales necesarios:** Trazado de carriles bici, red de calles, puntos de accidentes de tráfico (si hay datos abiertos disponibles) y ubicación de estaciones de transporte público.
- **Aportación del equipo:** Creación de un índice de conectividad espacial y desarrollo de un mapa interactivo web (`Folium`, `Kepler.gl` o `Leaflet`) para presentar los resultados de forma visual y atractiva.
- **Impacto Smart City:** Movilidad activa y seguridad vial.

**Stack tecnológico recomendado**

- **Para el análisis geoespacial:** Python (`GeoPandas`, `Shapely`, `NetworkX`, `Rasterio`) o software de SIG de escritorio (`QGIS`).
- **Para la visualización:** Herramientas web interactivas como `Kepler.gl` o mapas con `Folium` para que el proyecto luzca muy profesional en una presentación.

**NOTAS de la sesión 0:**

- No proyectos que partan de una tecnología, sino de una necesidad —> merece la pena ser resuelta y que aporta de nuevo?
- Partimos de un buen conocimiento del problema o partes del problema ( deberíamos al menos uno o varios del grupo), es un producto no un ejercicio académico.
- Encuestas? Búsqueda de un problema real antes de empezar el producto concreto? Entrevistas?Estudio de mercado sobre el problema a resolver? No está reñido con la temática.
- Ninguna idea surge de “a ver qué sale” , siempre hay algo ya pensado por un problema, miembro del equipo o una necesidad.
- Ojo a “datasets” de Kaggle.

![image.png](Decisiones,%20apuntes,%20notas%20del%20proyecto/image.png)

Otros: 

- Os parece bien explicar o exponer ideass ( en nuestras reuniones de grupo para acordar y repartir tareas) usando excalidraw compartiendo patnalla o canvas ( imagina que hay mas alternativas) como lo veis?
- Para obtener los datos: os surge alguna idea? deberíamos scrapear?, hay bases de datos oficiales de los datos que queremos obtener ( una vez obtengamos la idea concreta del proyecto)? Hay que investigar si es necesario. Los gobiernos suelen tener bases de datos con mucha info pública sobre todo EEUU podemos mirar ahi
- La IA generativa no toma decesiones!! Importante comentado por Julio
- **Tarea 1**: crear repo ( , readme? github? ) + 3 ideas—> con el feedback , decidimos.

![image.png](Decisiones,%20apuntes,%20notas%20del%20proyecto/image%201.png)

- Readme como ultimisima tarea para explicar como se despliega en cualquier dispositivo—>  Docker importante como contenedor y ¿despliegue?
- EL material aparece en la clase como material adjunto.
- EN la clase 0 están los enunciados de los ejercicios de todo el año como archivo adjunto.
- 2 semanas MAX para entregas de ejercicios tras cada módulo.

Ideas en comun

- **Fusión inteligente de sensores (Radar, Lidar, Óptico)** para generar mapas de conciencia situacional unificados en el campo de batalla.
- **Predicción de desastres naturales (inundaciones, desprendimientos)** mediante el análisis de series temporales de geodatos e índices de vegetación/humedad + RAFA + Joaquín
- **Sistema de optimización de rutas de transporte público bajo demanda** basado en predicciones de densidad de población y movilidad urbana en tiempo real. + RAFA.  ***
- **Optimización del flujo de tráfico en aeropuertos** mediante modelos predictivos de retrasos, gestión de colas en pistas y condiciones climáticas severas. + Joaquín
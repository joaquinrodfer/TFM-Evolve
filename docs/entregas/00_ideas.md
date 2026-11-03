# Equipo

| Nombre | Usuario de GitHub |
|---|---|
| Vicente Navas | [@VicenteNavas](https://github.com/vicentenr8) |
| Rafa García | [@RafaGarcía](https://github.com/Rafa-Garcia-Data) |
| Joaquín Rodríguez| [@JoaquínRodríguez](https://github.com/joaquinrodfer) |

# Fichas de proyecto: 3 ideas de producto

**Atributos de cada ficha:** problema · usuarios · cómo se resuelve hoy · datos y origen · MVP · medida de éxito.

## Resumen de un vistazo

| | Idea 1: Alerta de inundaciones | Idea 2: Rutas bajo demanda | Idea 3: Islas de calor y arbolado |
|---|---|---|---|
| **Usuario principal** | Protección civil municipal | Operadores y consorcios de transporte | Técnicos municipales de urbanismo y medio ambiente |
| **Qué entrega** | Probabilidad de crecida por tramo, con semáforo y explicación | Mapa de demanda y propuesta de rutas/paradas | Mapa de puntos calientes sin árboles y ranking de zonas prioritarias |
| **Dato clave** | Caudales SAIH + lluvia + humedad del suelo | Demanda real (validaciones) y posición de buses | LST de Landsat + inventario de arbolado |
| **Mayor riesgo** | Pocos eventos extremos para entrenar y validar | Acceso a datos de demanda (ticketing) | Pocos días despejados y correlación ≠ causalidad |
| **Madurez de datos** | Media (formatos distintos por cuenca) | Media-baja (la demanda no es pública) | Alta (casi todo abierto) |
| **Línea base** | Meteoalerta y EFAS/GloFAS | Horarios y frecuencias actuales | Criterio actual del ayuntamiento |

> Las filas de madurez y riesgo son una valoración inicial nuestra, pendiente de revisar.

---

## Idea 1: Alerta temprana de inundaciones a escala local

### Problema
Los ayuntamientos y la protección civil reciben avisos meteorológicos generales (por provincia o comarca), pero no una estimación del riesgo para **tramos concretos** de su municipio. Eso dificulta decidir con antelación si cortar una vía, avisar a una urbanización o movilizar recursos.

Un aviso de zona no dice qué tramo se va a desbordar ni con cuánta antelación. Hay literatura que evalúa si los umbrales de Meteoalerta (AEMET) se ajustan a los daños reales por municipio (estudio en la Comunitat Valenciana, publicado en el repositorio de AEMET), lo que indica que el desajuste entre aviso y daño local es un problema reconocido.

La brecha es mayor donde no hay sensores: las estaciones de aforo cubren pocos puntos de los ríos, y ramblas, barrancos y afluentes pequeños suelen carecer de ellas. Ahí un aviso local sería más útil y hoy no hay ninguna medición.

### Usuarios
- **Principal:** técnico/a de protección civil o emergencias de un ayuntamiento o mancomunidad/comarca. Segundo círculo: Policía Local y bomberos, que ejecutan cortes y evacuaciones.
- **Secundario:** responsables de infraestructuras en zonas inundables (carreteras, campings, polígonos).
- **Beneficiario final:** vecinos de zonas inundables.

### Cómo se resuelve hoy

| Herramienta | Qué devuelve | Qué no devuelve |
|---|---|---|
| **AEMET Meteoalerta** | Avisos por zona (verde/amarillo/naranja/rojo) con umbrales de precipitación en 1 h y 12 h | Riesgo hidrológico por tramo, probabilidad ni explicación local |
| **SAIH de cada confederación** | Niveles, caudales y lluvia en tiempo real (en el Ebro, cada 15 min) e históricos descargables | Predicción. Solo cubre puntos con estación y el dato en tiempo real es provisional hasta validarse |
| **SNCZI** (Cartografía de Zonas Inundables) | Mapas estáticos para periodos de retorno de 10, 50, 100 y 500 años | Nada en tiempo real ni enlace con la previsión |
| **EFAS / GloFAS** (Copernicus) | Previsión de crecidas fluviales y repentinas a escala europea, con probabilidades | Está pensado como complemento a los sistemas nacionales y regionales, a escala de cuenca, no de tramo municipal |
| **Planes de emergencia** (METEOCAM, Inuncam, etc.) | Protocolo de activación por fases | Estimación de riesgo local: se activan por los avisos |

**EFAS es el competidor más serio y, a la vez, la línea base natural con la que comparar.**

### Propuesta de valor
Integrar datos hidrometeorológicos reales y locales, entrenar y validar un modelo con histórico y devolver **probabilidades calibradas** por tramo.

**Idea clave: predecir en cualquier punto, sin estación de aforo.** Las estaciones se usan solo para entrenar y validar: el modelo aprende la relación entre lluvia, humedad del suelo, terreno y crecida donde hay medidas reales. Después se aplica con variables que existen en cualquier lugar (lluvia, humedad del suelo, pendiente, tamaño de cuenca), sin sensor en el punto. Eso permite escalar a cualquier zona de España.

### Producto
Un panel con mapa (GIS) donde, para cada tramo del cauce, se muestra:
1. Probabilidad de superar un umbral de crecida en las próximas horas.
2. Semáforo derivado de esa probabilidad.
3. Por qué: los factores que más empujan la alerta (lluvia acumulada, humedad previa, pendiente, tamaño de cuenca).
4. Qué hay expuesto en la zona inundable asociada (cruce con SNCZI: carreteras, edificios, población).
5. Un texto breve, redactado con IA generativa, que explica lo anterior.

### Datos y posibles fuentes (pendiente de revisión)

| Fuente | Aportación | Estado y detalles | Riesgo |
|---|---|---|---|
| **SAIH** (caudal, nivel, pluviómetros) | Etiquetas (crecidas) y lluvia de alta frecuencia | Cada confederación tiene su web. En el Ebro hay descarga de históricos en "datos a la carta" y datos cada 15 min | Formato y condiciones de reutilización distintos por cuenca |
| **Anuarios de Aforos** (MITECO, red SAIH-ROEA) | Caudales históricos validados | Publicados por año hidrológico | Resolución menor |
| **AEMET OpenData** | Observación, predicción, avisos | API key gratuita. La observación horaria solo da una ventana de unas 24 h (sin histórico); los valores diarios se piden en rangos de hasta 6 meses | Límite de peticiones y caídas intermitentes |
| **SNCZI** | Zonas inundables T=10, 50, 100 y 500 años | Shapefile y servicios ATOM / OGC API Features | Cobertura variable según los estudios de cada cuenca |
| **EFAS / GloFAS** (Copernicus EWDS) | Previsión y reanálisis histórico, como línea base o variable de entrada | Conjuntos históricos y reforecasts en el Early Warning Data Store. El acceso en tiempo real puede requerir solicitud | Resolución espacial gruesa para tramos pequeños |
| **Humedad del suelo, Sentinel-1** (Copernicus Land Monitoring) | Humedad superficial a 1 km | Unos 5 cm de profundidad, paso de 1,5-4 días. Error típico de ~0,07 m³ en validación global, peor en bosque y relieve fuerte. Alternativa a verificar: ERA5-Land | Puede enmascarar zonas de montaña |
| **Modelo digital del terreno, usos del suelo** | Pendiente, tipo de suelo, cobertura | Por verificar | Por evaluar |
| **Registro de daños o incidencias** | Etiquetas alternativas y validación | Por verificar | Por evaluar |

### MVP
- **Alcance:** varias cuencas con estaciones de aforo (las suficientes para separar entrenamiento y validación), un solo tipo de evento (crecida fluvial) y un modelo que da la probabilidad de superar un umbral de crecida en las próximas *N* horas **sin usar el caudal medido como entrada**.
- **Entrega:** mapa sencillo con semáforo y explicación.

### Medida de éxito
**Validación**
- **Espacial:** entrenar con unas estaciones y evaluar en cuencas que el modelo no ha visto, tratándolas como si no tuvieran sensor.
- **Temporal:** además, evaluar en episodios posteriores a los de entrenamiento, para evitar fugas de información.

**Líneas base:** (1) umbral de lluvia tipo Meteoalerta, (2) EFAS/GloFAS cuando haya datos, (3) climatología.

**Métricas:** sensibilidad (eventos detectados), falsas alarmas, antelación media del aviso y calibración de las probabilidades.

**Criterio de éxito:** mejorar a las líneas base en cuencas no vistas. El objetivo numérico se fija tras hablar con usuarios, según el error que toleren más. Se espera menos precisión que usando el caudal como entrada, y eso forma parte del planteamiento.

### Riesgos y supuestos a validar
- **Pocos eventos extremos:** las crecidas son raras, así que el conjunto de entrenamiento está muy desbalanceado y la validación puede ser poco estable.
- **Datos de lluvia de entrada:** AEMET OpenData no da histórico horario, así que hay que decidir de dónde sale la lluvia histórica (pluviómetros SAIH, reanálisis u otra fuente) y qué previsión de lluvia se usa para las próximas horas.
- **Etiquetas:** definir "crecida" por umbral de caudal no equivale a daño real; conviene contrastar con el registro de incidencias.
- **Responsabilidad:** se presenta como apoyo a la decisión y no sustituye a los avisos oficiales.

---

## Idea 2: Optimización de rutas de transporte público bajo demanda

> **Condición previa:** revisar la disponibilidad de *Floating Car Data* en tiempo real. Si no hay, el MVP puede hacerse con datos históricos, simulando el proceso sobre un dataset con frecuencia temporal razonable.

### Problema
Las líneas de transporte público tradicionales (autobuses lanzadera o interurbanos que conectan zonas dormitorio con grandes núcleos laborales) operan con horarios y rutas rígidos y prefijados. Esto genera dos ineficiencias:
- **Horas valle:** autobuses casi vacíos, con gasto de recursos y emisiones innecesarias.
- **Horas punta:** esperas y saturación por una mala previsión de la demanda real.

### Usuarios
- **Pasajeros:** trabajadores, estudiantes y residentes que planifican sus desplazamientos diarios.
- **Operadores y gestores públicos:** empresas de autobuses y consorcios que necesitan dimensionar flota, reducir costes y ajustar frecuencias.

### Cómo se resuelve hoy
Se basa en estimaciones históricas, hojas de cálculo, encuestas de movilidad poco frecuentes y la intuición de los planificadores. Los horarios apenas se adaptan a imprevistos (eventos, meteorología adversa, picos de demanda), lo que genera desconcierto en el pasajero y baja rentabilidad para la empresa. No hay trazabilidad ni anticipación.

Para el pasajero existen apps como Google Maps, Citymapper y Moovit, que muestran tiempos y posición en vivo pero **no adaptan la oferta a la demanda**.

### Datos y posibles fuentes

| Tipo | Contenido | Origen |
|---|---|---|
| **Geoespacial** | Paradas, redes de carreteras y líneas | QGIS, OpenStreetMap, GTFS |
| **Demanda y ticketing** | Validaciones de tarjetas de transporte y cancelaciones | Operadores y consorcios (**ver riesgo abajo**) |
| **Externos** | Meteorología, calendarios laborales y escolares, flujos de población y tráfico | AEMET, calendarios oficiales, fuentes de tráfico |

**Comparativa de fuentes (caso Madrid)**

| Fuente | Tipo | Tiempo real | Notas |
|---|---|---|---|
| [CRTM](https://www.crtm.es/) | GTFS estático (líneas, paradas, horarios) | Parcial | Lo más estable para empezar; base de cualquier proyecto de rutas |
| [EMT Madrid OpenData](https://opendata.emtmadrid.es/) | Llegadas a parada, incidencias | Sí, cuando está disponible | Muy útil para el caso; muchos endpoints requieren registro |
| [Datos Abiertos Ayto. de Madrid](https://datos.madrid.es) | Tráfico, sensores, parkings, incidencias, calidad del aire | Depende | Buscar por "tráfico", "incidencia" o "sensor"; hay históricos y datasets que se actualizan |
| [Mobility Labs Madrid](https://mobilitylabs.emtmadrid.es/) | Proyectos experimentales | Variable | Revisar su documentación: a veces expone pruebas y otras redirige a EMT/CRTM |
| [Mobility Database](https://mobilitydatabase.org/) | Agregador mundial de GTFS/GTFS-RT | Mixto | Para localizar un GTFS-RT publicado sin adivinar URLs |

**Datos en vivo y cobertura**
- **EMT Madrid (urbano):** API de tiempo real en Mobility Labs. Con API key se consumen GTFS-RT o la API REST (Swagger) con la posición de los autobuses.
- **CRTM (interurbano):** publica el posicionamiento en vivo como feeds dinámicos en su Portal de Datos Abiertos.
- **Tráfico en Madrid:** Etraffic (DGT Infocar) para retenciones, obras y cortes; DGT Mapa de movilidad; Informo (Ayto. de Madrid) con intensidad cada 5 min; ViaMichelin para atascos y velocidad.
- **NAP de la DGT:** [nap.dgt.es](https://nap.dgt.es), con bases de datos de tráfico por ciudad.

### Escalabilidad a otras ciudades
Estas ciudades tienen portal local propio equivalente al de Madrid:

| Ciudad | Fuente |
|---|---|
| Barcelona | Trànsit en temps real del Ajuntament e incidencias del Servei Català de Trànsit |
| Valencia | Tráfico en Tiempo Real del Ayuntamiento, con cámaras |
| Sevilla | Centro de Gestión de la Movilidad |
| Zaragoza | Plano interactivo de tráfico, incidencias de carriles y ocupación en tiempo real |
| Málaga | Centro de Control de Tráfico, con estado de vías y cámaras |
| Bilbao | Canales de movilidad y Trafikoa (Gobierno Vasco) |
| Murcia | Tráfico en directo e incidencias en el portal municipal |
| Palma de Mallorca | Mapa interactivo de circulación |
| Valladolid | Mapa de tráfico del Centro de Movilidad Urbana |
| Alicante | Mapas de incidencias y cortes programados |
| A Coruña y Vigo | Mapas interactivos del estado de la red viaria interna |

### MVP
- **Alcance:** cuadro de mandos o app web ligera (Streamlit con Folium/GeoPandas) sobre **un distrito** lo bastante grande para abarcar las *features* del proyecto.
- **Funciones:**
  1. Mapa interactivo que compara las rutas actuales con la demanda real estimada por zonas. Es el verdadero reto técnico y de fuentes.
  2. Modelo de ML sencillo (o clustering espacial) que sugiera optimizar paradas o agrupar pasajeros en rutas dinámicas para un corredor o franja horaria concreta.

### Medida de éxito

**Técnica**
- Precisión del modelo de demanda: RMSE o MAE al predecir pasajeros por franja horaria y parada.
- Calidad del servicio simulado: % de solicitudes atendidas, espera media y ratio desvío/tiempo directo.
- Eficiencia: km por pasajero, % de trayectos dentro de la ventana horaria y tiempo de cómputo (< X s para N solicitudes).

**De producto** (frente a la operación actual como línea base)
- Reducción porcentual de km en vacío (ahorro de combustible y CO₂).
- Disminución del tiempo medio de espera.
- Coste por pasajero-km y ocupación media.
- Umbrales objetivo de ejemplo: ≥ 95 % de solicitudes atendidas, −20 % de espera, resolución en < 60 s.

### Riesgos y supuestos a validar
- **Demanda no pública:** los datos de validaciones suelen ser del operador. Sin acuerdo con uno, la demanda hay que **estimarla** (flujos de población, tráfico, calendarios) y el modelo sería un *proxy*. Hay que decirlo claro en el MVP.
- **Fuente de verdad del MVP:** decidir pronto entre tiempo real (si hay Floating Car Data) y simulación histórica.
- **Línea base sin definir:** hace falta fijar con qué se compara (horario actual de una línea concreta) para que "−20 % de espera" signifique algo.
- **Sesgo hacia Madrid:** las fuentes citadas son de Madrid; la escalabilidad a otras ciudades depende de que publiquen GTFS y GTFS-RT.

---

## Idea 3: Islas de calor urbanas (UHI) y su relación con el arbolado

### Problema
Las ciudades son varios grados más calurosas que su entorno, y el calor extremo ya provoca mortalidad, gasto energético y pérdida de calidad de vida. El arbolado es una de las soluciones más baratas, pero los ayuntamientos no saben **dónde plantar para obtener más enfriamiento por euro**. Hoy se decide con criterio político o con datos fragmentados, y casi nunca se cuantifica el efecto real de cada árbol o zona verde.

### Usuarios
- **Principal:** ayuntamientos (urbanismo, medio ambiente, parques y jardines).
- **Salud pública y protección civil:** planes de calor y localización de zonas vulnerables.
- **Consultoras de urbanismo y sostenibilidad:** estudios para administraciones.
- **ONG y asociaciones vecinales:** para reclamar actuaciones en sus barrios.
- **Secundarios:** investigadores y periodistas de datos.

### Cómo se resuelve hoy
- Informes puntuales de universidades o consultoras: caros y desactualizados a los pocos años.
- Mapas de temperatura superficial hechos a mano con QGIS o Google Earth Engine por perfiles muy especializados.
- Inventarios de arbolado en Excel o GIS municipal, sin relación con la temperatura.
- Planes de calor genéricos que identifican "barrios vulnerables" con datos socioeconómicos, pero sin análisis térmico fino.
- Herramientas internacionales (Copernicus, NASA) que existen pero son poco accesibles para un técnico municipal.

### Datos y posibles fuentes

| Dato | Fuente |
|---|---|
| Temperatura superficial (LST) | Landsat 8/9 (USGS/NASA, 30-100 m), ECOSTRESS (NASA, ~70 m), MODIS (1 km, solo contexto) |
| Vegetación (NDVI, EVI) | Sentinel-2 (Copernicus, 10 m) |
| Cobertura del suelo e impermeabilidad | Copernicus Urban Atlas, Imperviousness Density, CORINE |
| Copa arbórea | Copernicus Street Tree Layer, LiDAR del PNOA (IGN), alturas de vegetación |
| Inventario de arbolado | Portales de datos abiertos municipales (Madrid, Barcelona, Sevilla, Valencia, etc.) |
| Edificación y morfología urbana | Catastro (INSPIRE), OpenStreetMap, LiDAR PNOA |
| Temperatura del aire (validación) | AEMET, estaciones municipales, redes ciudadanas |
| Reanálisis meteorológico | ERA5-Land (Copernicus) |
| Vulnerabilidad social | INE (censo, renta, edad por sección censal) |
| Terreno | Modelo digital del terreno (CNIG) |

### MVP
**Alcance:** una sola ciudad piloto con inventario de arbolado abierto y una sola temporada de verano.

1. Descargar imágenes Landsat de días despejados de verano y calcular la LST media por píxel.
2. Calcular NDVI y % de copa arbórea con Sentinel-2 y el inventario municipal.
3. Agregar por malla (100×100 m) o sección censal.
4. Modelar LST ~ cobertura arbórea + superficie impermeable + morfología + distancia al agua (regresión o random forest con SHAP para interpretar).
5. Generar un mapa de **"puntos calientes sin árboles"** y un ranking de zonas prioritarias, cruzado con población vulnerable (mayores de 65, renta baja).
6. Entregar un dashboard simple (Streamlit o Folium) con filtros por barrio y un informe breve con el efecto estimado ("+10 % de copa ≈ −X °C").

**Fuera del MVP:** simulación de escenarios futuros, tiempo real, multi-ciudad y modelos microclimáticos (ENVI-met).

### Medida de éxito

**Técnica**
- El modelo explica una parte relevante de la variación de LST (p. ej. R² > 0,6 en validación espacial).
- Efecto estimado de la copa arbórea coherente con la literatura (típicamente 1-4 °C de diferencia en LST).
- Validación cruzada con estaciones de aire o campañas de medición.

**De producto**
- Un técnico municipal identifica en menos de 10 minutos las 10 zonas prioritarias de plantación.
- Al menos 1-2 entidades (ayuntamiento, ONG o consultora) lo validan como útil en un piloto.
- Las recomendaciones coinciden con el criterio actual o aportan algo nuevo.

**De impacto (medio plazo)**
- Se incorpora a una decisión real de plantación o de plan de calor.
- Reducción medible de LST en zonas intervenidas frente a zonas de control.

### Riesgos y supuestos a validar
- **Pocas imágenes útiles:** Landsat pasa cada pocos días y la nubosidad reduce las escenas válidas en una sola temporada; conviene fijar cuántas se necesitan.
- **LST ≠ temperatura del aire:** la temperatura superficial no es lo que siente la gente; por eso la validación con estaciones de aire es clave.
- **Correlación vs. causalidad:** el modelo mide asociación entre copa y LST; "+10 % de copa ≈ −X °C" debe presentarse como estimación con intervalo, no como efecto garantizado.
- **Autocorrelación espacial:** celdas vecinas se parecen, así que la validación espacial debe dejar zonas enteras fuera (no celdas sueltas) para que el R² no salga inflado.
- **"Por euro invertido":** el problema habla de coste-eficacia, pero el MVP no incluye costes de plantación; o se añade un dato de coste, o el ranking se plantea solo como impacto por zona.

---

## Siguientes pasos
1. Recoger el feedback de por idea.
2. Resolver los puntos marcados como "por verificar" en la idea elegida.
3. Concretar el objetivo numérico del criterio de éxito con usuarios reales.

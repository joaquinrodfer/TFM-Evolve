# Fichas proyectos

<aside>
💡

Julio nos pide 3 ideas de producto con los siguientes atributos:

- Problema
- Usuarios
- Cómo se resuelve hoy
- Posibles datos y su origen
- MVP
- Medida de éxito

Nos devolverá feedback por idea y ya decidimos nosotros la que más nos convenza.

</aside>

# Idea 1: Alerta temprana de inundaciones a escala local

## Problema o necesidad

Los ayuntamientos y la protección civil de zonas con riesgo de inundación reciben avisos meteorológicos generales (por provincia o comarca), pero no una estimación del riesgo para tramos concretos de su municipio. Eso dificulta decidir con antelación si cortar una vía, avisar a una urbanización o movilizar recursos.

Un aviso de zona no dice qué tramo concreto se va a desbordar ni cuánto antes. Existe literatura que evalúa si los umbrales de Meteoalerta (AEMET) se ajustan a los daños reales por municipio (estudio en la Comunitat Valenciana publicado en el repositorio de AEMET), lo que indica que el ajuste entre aviso y daño local es un problema reconocido.

La brecha es mayor donde no hay sensores: las estaciones de aforo solo cubren unos pocos puntos de los ríos, y ramblas, barrancos y afluentes pequeños suelen carecer de ellas. Ahí es donde un aviso local resultaría más útil y donde hoy no hay ninguna información medida.

## Usuarios afectados

- **Usuario principal**: técnico/a de protección civil o emergencias de un ayuntamiento o una mancomunidad/comarca (y, como segundo círculo, jefatura de Policía Local o bomberos que ejecutan cortes y evacuaciones)
- **Usuario secundario**: responsable de infraestructuras como carreteras, campings, polígonos, etc. en zonas inundables
- **Beneficiario final**: vecinos de zonas inundables.

## Cómo se resuelve hoy

| Herramienta | Qué devuelve | Qué no devuelve |
| --- | --- | --- |
| AEMET Meteoalerta | Avisos por zona (verde/amarillo/naranja/rojo) con umbrales de preciciptación en 1 h y 12 h | Riesgo hidrológico por tramo; probabilidad; explicación local |
| **Sistemas Automáticos de Información Hidrológica (**SAIH) de cada confederación | Niveles, caudales y lluvia en tiempo real (en el Ebro, datos cada 15 min) y descarga de históricos | Predicción; solo cubre puntos con estación; los datos en tiempo real son provisionales hasta que se validan |
| Sistema Nacional de Cartografía de Zonas Inundables (SNCZI) | Mapas estáticos de zonas inundables para periodos de 10, 50, 100 y 500 años | Nada en tiempo real; no se enlaza con la previsión |
| *European Flood Awareness System* (EFAS) / *Global Flood Awareness System* (GloFAS) de Copernicus | Previsión de crecidas fluviales y de crecidas repentinas en escala europea (EFAS), con probabilidades | Está pensado como información complementaria a los sistemas nacionales y regionales, a escala de cuenca, no de tramo municipal |
| Planes de emergencia (METEOCAM, Inuncam, etc.) | Protocolo de activación por fases | No aportan estimación de riesgo local; se activan por los avisos |

EFAS es el competidor más serio y, a la vez, una línea base natural para comparar.

El valor está en integrar datos hidrometeorológicos reales y locales, entrenar y validar un modelo con histórico y devolver probabilidades calibradas.

## Cómo se podría mejorar

El enfoque es realizar predicción en cualquier punto, sin estación de aforo. Estas se usarían solo para entrenar y validar: el modelo aprende la relación entre lluvia, humedad del suelo, terreno y crecida donde hay medidas reales. Después, se aplica con variables que existen en cualquier lugar (lluvia, humedad del suelo, terreno y crecida donde hay medidas reales), sin necesitar un sensor en el punto. Esto es lo que permite escalar a cualquier zona de España.

### Producto

Un panel con mapa (software GIS) donde, para cada tramo del cauce, se muestra:

1. Probabilidad de superar un umbral de crecida en las próximas horas.
2. Semáforo derivado de esa probabilidad.
3. Por qué (los factores que más empujan la alerta: lluvia acumulada, humedad previa, pendiente y tamaño de la cuenca).
4. Qué hay expuesto en la zona inundable asociada (cruce con SNCZI: carreteras, edificios, población).
5. Un texto breve redactado por IA generativa explicando los puntos anteriores.

## Datos y posibles fuentes (pendiente de revisión)

| Fuente | Aportación | Estado y detalles  | Riesgo |
| --- | --- | --- | --- |
| SAIH de la confederación (caudal, nivel, pluviómetros) | Etiquetas (crecidas) y lluvia de alta frecuencia | Cada confederación tiene su web. En el Ebro, la web permite descarga de históricos en “datos a la carta” y publica datos cada 15 min.  | Podrían tener formato y condiciones de reutilización distintos por cuenca |
| Anuarios de Aforos (MITECO, red SAIH-ROEA) | Caudales históricos validados. | Publicados por año hidrológico. | Resolución menor. |
| AEMET OpenData | Observación, predicción, avisos. | Requiere API key gratuita. La observación horaria solo devuelve una ventana reciente (unas 24 h) y no permite recuperar histórico; los valores diarios se piden en rangos de hasta 6 meses. | Hay límite de peticiones y, al parecer, caídas intermitentes. |
| SNCZI | Zonas inundables T=10, 50, 100 y 500 años. | Descarga en shapefile y servicios ATOM / OGC API Features. | Cobertura variable según estudios disponibles en cada cuenca. |
| EFAS /GloFAS (Copernicus EWDS) | Previsión de crecidas y reanálisis histórico para usar como línea base o variable de entrada. | Conjuntos históricos de previsión y reforecasts en el Early Warning Data Store. El acceso a previsiones en tiempo real y reforecasts puede requerir solicitud. | Resolución espacial gruesa para tramos pequeños. |
| Humedad del suelo, Sentinel-1 (Copernicus Land Monitoring) | Humedad superficial a 1 km. | Producto de unos 5 cm de profundidad, con paso cada 1,5-4 días. Error típico de unos 0,07 m³ en validación global, y peor concordancia en bosque y relieve fuerte. Alternativa por verificar: ERA5-Land. | Puede enmascarar zonas de montaña. |
| Modelo digital del terreno, usos del suelo | Pendiente, tipo de suelo, cobertura.  | Por verificar |  |
| Registro de daños o incidencias | Etiquetas alternativas y validación | Por verificar |  |

## MVP

**Alcance**: varias cuencas con estaciones de aforo (las necesarias para separar entrenamiento y validación), un tipo de evento (crecida fluvial) y un modelo que da la probabilidad de superar un umbral de crecida en las próximas N horas sin usar el caudal medido como entrada, mostrada en un mapa sencillo con explicación.

## Cómo se mediría el éxito

**Validación dejando ciertas cuencas fuera**: entrenar con unas estaciones y evaluar en estaciones de cuencas que el modelo no ha visto, tratándolas como si no tuvieran sensor. Con partición también temporal (episodios posteriores a los de entrenamiento) para evitar fugas de información.

**Líneas base sobre las que comparar el modelo**: (1) umbral de lluvia tipo Meteoalerta, (2) EFAS/GloFAS cuando haya datos, (3) climatología.

**Métricas**: sensibilidad (eventos detectados), falsas alarmas, antelación media del aviso y calibración de las probabilidades.

**Criterio de éxito**: mejora frente a las líneas base en cuencas no vistas. Se fija un objetivo numérico tras hablar con usuarios, según el error que toleren más. Se espera menos precisión que con el caudal como entrada, y es parte del planteamiento.

# **Idea 2: Sistema de optimización de rutas de transporte público bajo demanda**

Importante revisar disponibilidad de Floating Car Data en tiempo real (si no, para un simple MVP se puede hacer con datos históricos y simular el proceso a través de un dataset con una frecuencia temporal decente).

## Problema

Las líneas de transporte público tradicionales (como autobuses lanzadera o interurbanos que conectan zonas residenciales dormitorio con grandes núcleos laborales) suelen operar con horarios y rutas rígidas y prefijadas. Esto genera dos ineficiencias: autobuses circulando casi vacíos en horas valle (malgasto de recursos y emisiones innecesarias) y tiempos de espera o saturación en horas punta debido a una mala previsión de la demanda real.

## Usuarios

- Usuarios finales (ciudadanos/pasajeros): trabajadores, estudiantes o residentes que necesitan planificar sus desplazamientos diarios de forma eficiente.
- Operadores de transporte y gestores públicos: empresas de autobuses o consorcios de transporte que necesitan dimensionar la flota, reducir costes y ajustar las frecuencias en tiempo real.

## Cómo se resuelve hoy

Actualmente se basa en estimaciones históricas, Excel, encuestas de movilidad muy poco frecuentes y la intuición de los planificadores de tráfico. Los horarios apenas se adaptan a imprevistos (eventos, condiciones meteorológicas adversas o picos de demanda), lo que provoca desconcierto en el pasajero y baja rentabilidad para la empresa. No hay trazabilidad ni anticipación.

## Posibles datos y su origen

- Datos geoespaciales y de tráfico: coordenadas de paradas y capas vectoriales de redes de carreteras y líneas (QGIS / OpenStreetMap / GTFS — General Transit Feed Specification).
- Datos de demanda y ticketing: registros históricos de validaciones de tarjetas de transporte y cancelaciones de billetes previos.
- Datos externos: información meteorológica, calendarios laborales y escolares, y flujos de población y tráfico geolocalizados para predecir picos de movilidad.

### Fuentes de datos y comparativa

| Fuente | Tipo | Tiempo real | Notas |
| --- | --- | --- | --- |
| CRTM (Consorcio Regional de Transportes de Madrid) — https://www.crtm.es/ | GTFS estático (líneas, paradas, horarios) | Parcial | Lo más estable para empezar. Suele ser la base para cualquier proyecto de rutas. |
| EMT Madrid OpenData — https://opendata.emtmadrid.es/ | Llegadas a parada, incidencias, etc. | Sí (cuando está disponible) | Muy útil para el caso. Requiere registro en muchos de sus endpoints. |
| Datos Abiertos Ayuntamiento de Madrid — https://datos.madrid.es | Tráfico, sensores, parkings, incidencias, calidad del aire… | Depende | Buscar por «tráfico», «incidencia» o «sensor». Algunos datasets son históricos y otros se actualizan periódicamente. |
| Mobility Labs Madrid — https://mobilitylabs.emtmadrid.es/ | Proyectos/experimentales | Variable | Conviene revisar su documentación: a veces expone pruebas y otras redirige a EMT/CRTM. |
| MobilityData / Mobility Database — https://mobilitydatabase.org/ | Agregador GTFS/GTFS-RT mundial | Mixto | Ideal para ver si hay un GTFS-RT publicado para CRTM/EMT sin tener que adivinar URLs. |

### Datos en vivo y cobertura

- EMT Madrid (líneas urbanas): API de tiempo real en EMT Mobility Labs; con una API key se consumen GTFS-RT o la API REST (Swagger) con la posición exacta de los autobuses.
- CRTM (líneas interurbanas): el Consorcio Regional de Transportes publica el posicionamiento en vivo como feeds dinámicos en su Portal de Datos Abiertos.
- The Mobility Database: referencia para localizar GTFS-RT de cualquier ciudad del mundo.
- Para usuarios finales: Google Maps (capa de transporte público con tiempos e icono del bus en vivo), Citymapper y Moovit.
- Tráfico en Madrid: Etraffic (DGT Infocar) para retenciones, obras y cortes; DGT Mapa de movilidad para puntos kilométricos y restricciones; Informo (Ayuntamiento de Madrid) con intensidad de tráfico cada 5 minutos; ViaMichelin para atascos y velocidad de circulación.
- Otras fuentes: NAP de la DGT (https://nap.dgt.es) con bases de datos de tráfico por ciudad y el portal de datos abiertos del Ayuntamiento de Madrid.

### Escalabilidad: otras ciudades

Las principales ciudades españolas que disponen de un portal local propio equivalente al de Madrid son:

- Barcelona: portal de Trànsit en temps real del Ajuntament y las incidencias del Servei Català de Trànsit.
- Valencia: portal de Tráfico en Tiempo Real del Ayuntamiento, con cámaras web distribuidas por la ciudad.
- Sevilla: portal del Centro de Gestión de la Movilidad del Ayuntamiento.
- Zaragoza: plano interactivo de tráfico, incidencias de carriles y ocupación en tiempo real.
- Málaga: Centro de Control de Tráfico, con estado de vías principales y cámaras.
- Bilbao: canales de movilidad y el soporte de la Dirección de Tráfico del Gobierno Vasco (Trafikoa).
- Murcia: datos de tráfico en directo e incidencias en el portal municipal.
- Palma de Mallorca: mapa interactivo de circulación del área de movilidad.
- Valladolid: mapa de tráfico interactivo actualizado por el Centro de Movilidad Urbana.
- Alicante: mapas locales de incidencias y cortes de tráfico programados.
- A Coruña y Vigo: mapas interactivos con el estado de la red viaria interna.

## MVP

**Alcance**: un cuadro de mandos interactivo o una aplicación web ligera (Streamlit con Folium/GeoPandas) aplicada a un distrito lo suficientemente grande para abarcar las features del proyecto, que permita:

1. Visualizar en un mapa interactivo las rutas actuales frente a la demanda real estimada por zonas geográficas (el verdadero reto de fuentes y técnico).
2. Un modelo de Machine Learning sencillo (o un clustering espacial) que sugiera la optimización de paradas o la agrupación de pasajeros en rutas dinámicas para un corredor o franja horaria concreta.

## Medida de éxito

**Técnica**

- Precisión del modelo de demanda: RMSE o MAE al predecir pasajeros por franja horaria y estación.
- % de solicitudes atendidas, espera promedio y ratio desvío/tiempo directo.
- Km por pasajero, % de trayectos dentro de la ventana horaria y tiempo de cómputo (< X s para N solicitudes).

**De producto**

- Reducción porcentual de los kilómetros recorridos en vacío por la flota (ahorro de combustible y emisiones de CO₂).
- Disminución del tiempo medio de espera del usuario.
- Coste por pasajero-km y ocupación media, con umbral objetivo (p. ej. ≥95% de solicitudes atendidas, −20% de espera, solve < 60 s).

# **Idea 3: Estudio de Islas de Calor Urbanas (UHI) y su Relación con el Arbolado**

### Problema

Las ciudades son varios grados más calurosas que su entorno, y el calor extremo ya causa mortalidad, gasto energético y pérdida de calidad de vida. El arbolado es una de las soluciones más baratas, pero los ayuntamientos no saben **dónde plantar para obtener el mayor enfriamiento por euro invertido**. Hoy se decide con criterio político o con datos fragmentados, y casi nunca se cuantifica el efecto real de cada árbol o zona verde.

### Usuarios

- **Ayuntamientos** (urbanismo, medio ambiente, parques y jardines).
- **Técnicos de salud pública y protección civil**, para planes de calor y localización de zonas vulnerables.
- **Consultoras de urbanismo y sostenibilidad**, que hacen estudios para administraciones.
- **ONG y asociaciones vecinales**, para reclamar actuaciones en sus barrios.
- **Investigadores y periodistas de datos**, como usuarios secundarios.

### Cómo se resuelve hoy

- Informes puntuales de universidades o consultoras, caros y desactualizados a los pocos años.
- Mapas de temperatura superficial hechos a mano con QGIS o Google Earth Engine por perfiles muy especializados.
- Inventarios de arbolado en Excel o GIS municipal, sin relación con la temperatura.
- Planes de calor genéricos que identifican “barrios vulnerables” con datos socioeconómicos, pero sin análisis térmico fino.
- Herramientas internacionales (p. ej. Copernicus o NASA) que existen, pero son poco accesibles para un técnico municipal.

### Posibles datos y su origen

| Dato | Fuente |
| --- | --- |
| Temperatura superficial (LST) | Landsat 8/9 (USGS/NASA, 30-100 m), ECOSTRESS (NASA, ~70 m), MODIS (1 km, solo contexto) |
| Vegetación (NDVI, EVI) | Sentinel-2 (Copernicus, 10 m) |
| Cobertura del suelo y superficies impermeables | Copernicus Urban Atlas, Imperviousness Density, CORINE |
| Copa arbórea | Copernicus Street Tree Layer, LiDAR del PNOA (IGN), alturas de vegetación |
| Inventario de arbolado | Portales de datos abiertos municipales (Madrid, Barcelona, Sevilla, Valencia, etc.) |
| Edificación y morfología urbana | Catastro (INSPIRE), OpenStreetMap, LiDAR PNOA |
| Temperatura del aire (validación) | AEMET, estaciones municipales, redes ciudadanas |
| Reanálisis meteorológico | ERA5-Land (Copernicus) |
| Vulnerabilidad social | INE (censo, renta, edad por sección censal) |
| Terreno | Modelo digital del terreno (CNIG) |

### MVP

**Alcance:** una sola ciudad piloto, con inventario de arbolado abierto, y una sola temporada de verano.

1. Descargar imágenes Landsat de días despejados de verano y calcular la LST media por píxel.
2. Calcular NDVI y porcentaje de copa arbórea con Sentinel-2 y el inventario municipal.
3. Agregar todo por malla (100x100 m) o sección censal.
4. Modelar la relación LST ~ cobertura arbórea + superficie impermeable + morfología + distancia al agua (regresión o random forest con SHAP para interpretar).
5. Generar un **mapa de “puntos calientes sin árboles”** y un ranking de zonas prioritarias, cruzado con población vulnerable (mayores de 65, renta baja).
    1. Entregarlo como dashboard simple (Streamlit o Folium) con filtros por barrio y un informe breve con el efecto estimado (“+10% de copa ≈ −X °C”).

**Fuera del MVP:** simulación de escenarios futuros, tiempo real, multi-ciudad, modelos microclimáticos (ENVI-met).

### Medida de éxito

**Técnica**

- El modelo explica una parte relevante de la variación de LST (p. ej. R² > 0,6 en validación espacial).
- Efecto estimado de la copa arbórea coherente con la literatura (típicamente 1-4 °C de diferencia en LST).
- Validación cruzada con estaciones de aire o campañas de medición.

**De producto**

- Un técnico municipal identifica en menos de 10 minutos las 10 zonas prioritarias de plantación.
- Al menos 1-2 entidades (ayuntamiento, ONG o consultora) lo validan como útil en una prueba piloto.
- Las recomendaciones coinciden o aportan algo nuevo frente a su criterio actual.

**De impacto** (a medio plazo)

- Se incorpora a una decisión real de plantación o de plan de calor.
- Reducción medible de LST en zonas intervenidas, comparada con zonas control.

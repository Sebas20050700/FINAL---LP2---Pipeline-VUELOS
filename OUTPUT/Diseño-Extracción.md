# 🛠️ Documento de Planeamiento y Diseño de Extracción
## Sistema de Inteligencia Aero-Meteorológica

Este documento detalla la estrategia técnica utilizada para la descarga, limpieza y combinación de datos multicanal (API Satelital + Base Terrestre SENAMHI).

---

### 1. 🎯 Objetivo del Diseño
El diseño busca eliminar la dependencia de una sola fuente de datos. Si el satélite (Visual Crossing) no reporta datos actuales, el sistema debe ser capaz de localizar automáticamente la infraestructura física más cercana en suelo peruano para validar las condiciones.

---

### 2. 🧬 Arquitectura de los Datos (El "Join" Espacial)

Para combinar las fuentes, se definió un flujo de **3 etapas de estructuración**:

#### A. Identificación y Geolocalización (Etapa 1)
* **Fuente:** API Visual Crossing.
* **Lógica:** Se utiliza el código ICAO del aeropuerto (ej. SPJC) + el sufijo ", Peru" para obtener las coordenadas maestras (Latitud/Longitud).
* **Salida:** Un DataFrame raíz que sirve como "llave" para las siguientes etapas.

#### B. Validación Forense (Etapa 3 - SENAMHI)
* **Fuente:** `MAESTRO_ESTACIONES_SENAMHI_GEO.csv` (Datos de estaciones terrestres).
* **Lógica de Combinación:** Se implementó un algoritmo de **Vecino más Cercano**. 
* **Cálculo:** Se utiliza la fórmula de Distancia Euclidiana multiplicada por un factor de corrección de $111.12$ para convertir grados geográficos en kilómetros reales.
  $$d = \sqrt{(lat_1 - lat_2)^2 + (lon_1 - lon_2)^2} \times 111.12$$

#### C. Consolidación de Resultados
* **Estructura final:** Se genera un archivo `Reporte_etapa_clima.csv` que fusiona:
    1. Datos de la API (Temperatura, Pronóstico).
    2. Datos de SENAMHI (Nombre de la estación validadora).
    3. Metadatos (Distancia de validación en KM).

---

### 3. 🧗 Manejo de Dificultades y Resiliencia
Durante el diseño se resolvieron los siguientes retos técnicos:

1.  **Valores Nulos (NaN):** Se observó que la API a veces devuelve `N/D`. El diseño incluye una función de limpieza que reemplaza estos nulos con `0.0` para no romper el pipeline.
2.  **Ambigüedad Geográfica:** Se corrigió el filtrado de ubicación para asegurar que las coordenadas pertenezcan a Perú y no a ubicaciones homónimas en el extranjero.
3.  **Precisión del Sensor:** El diseño prioriza la estación SENAMHI más cercana, permitiendo auditar la veracidad del satélite mediante la columna de "Distancia de Validación".

---

### 📊 Estructura del Producto Final (CSV)
El archivo de salida final cuenta con las siguientes columnas estructuradas:
* `aeropuerto_id`: Identificador único ICAO.
* `temp_c`: Temperatura validada.
* `estado_actual`: Condición reportada por el satélite.
* `VALIDADOR_TIERRA`: Estación física de SENAMHI asignada.
* `DIST_VALIDACIÓN`: Proximidad del sensor en kilómetros.

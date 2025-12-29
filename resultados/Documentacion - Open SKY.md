# 🛰️ Flujo Técnico: API_vuelos.ipynb (Etapa 2)

Este documento detalla el procesamiento interno del motor de telemetría **OpenSky Network**, el cual constituye la **Etapa 2** del pipeline.

## ⚙️ Arquitectura del Procesamiento
El script ejecuta un flujo cíclico de tres niveles para transformar datos brutos ADS-B en reportes operativos:

1.  **Ingesta Geofenced**: Captura masiva de estados de vuelo mediante la API de OpenSky, limitada estrictamente al espacio aéreo peruano mediante un Bounding Box.
2.  **Filtrado de Proximidad**: Cálculo de distancia geodésica en tiempo real entre las aeronaves detectadas y los puntos de control estratégicos: **Lima, Cusco, Arequipa e Iquitos**.
3.  **Clasificación de Riesgo**: Asignación de alertas automáticas según la cinemática del vuelo:
    * **🔴 CRÍTICO**: Aeronaves < 5000m y con tasa vertical negativa (Aproximación final).
    * **🟡 ADVERTENCIA**: Aeronaves en fase de descenso activo.
    * **🔵 INFO**: Aeronaves en fase de despegue o ascenso inicial.
    * **🟢 SEGURO**: Vuelos nivelados en altitud de crucero.



## 📂 Archivos Generados (Data Outputs)
Al finalizar la ejecución, el flujo exporta los siguientes archivos a la carpeta `data/processed/` o al directorio raíz para la integración de datos:

* **`TELEMETRIA_ETAPA2.csv`**: Detalle técnico de vuelos cercanos con su estado de alerta, altitud, velocidad y distancia calculada.
* **`REPORT_ETAPA2_OPENSKY.csv`**: Dashboard resumen que consolida la saturación de zonas, el estado operativo por aeropuerto y el cálculo de **ETA** (Tiempo Estimado de Llegada).
* **`RADAR_PERU_ACTUAL.csv`**: Listado procesado de aeronaves identificadas únicamente dentro del territorio nacional.

---
*Este flujo garantiza la **Realidad Operativa** del sistema al monitorear el tráfico aéreo de manera independiente a factores externos.*

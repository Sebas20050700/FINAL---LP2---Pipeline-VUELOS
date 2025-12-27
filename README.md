# ✈️ Pipeline de Análisis Aero-Meteorológico (Rama SENAMHI)
### Correlación entre Fenómenos Atmosféricos y Eficiencia en Rutas Aéreas Comerciales - Edición Perú

![Python](https://img.shields.io/badge/Python-3.8%2B-blue) ![Selenium](https://img.shields.io/badge/Selenium-Web%20Scraping-green) ![SENAMHI](https://img.shields.io/badge/Data-SENAMHI%20Perú-red) ![Status](https://img.shields.io/badge/Status-Operational-brightgreen)

## 📋 Descripción General
Esta rama (**`feature/senamhi-integration`**) implementa un módulo de validación meteorológica de alta precisión para el espacio aéreo peruano.

Sustituye la capa genérica (EcoWeather) por una **integración directa y forense con el SENAMHI** (Servicio Nacional de Meteorología e Hidrología del Perú), permitiendo distinguir matemáticamente entre **Lluvia**, **Nieve** y **Helada** en los Andes mediante datos de estaciones terrestres y satélites.

---

## 🏗️ Arquitectura del Pipeline

El sistema fusiona tres fuentes de datos para validar retrasos aéreos:

### 1. Tráfico Aéreo (OpenSky Network) 📡
* **Función:** Telemetría en vivo. Detecta patrones de espera, baja velocidad y altitud.
* **Cobertura:** Bounding Box del territorio peruano.

### 2. Contexto General (Visual Crossing) ☁️
* **Función:** Datos METAR generales (Viento, Visibilidad) para aeropuertos de origen/destino.

### 3. Validación Local (Módulo Custom SENAMHI) 🏔️
* **Función:** Capa de verificación de fenómenos extremos en tierra.
* **Técnica:** Web Scraping Forense y Análisis Vectorial.

---

## 🔧 Implementación Técnica (Lo que hace el código)

### A. Minería de Datos Forense (Scraping)
A diferencia de métodos tradicionales, este pipeline no lee el HTML visible.
* **Detector de API Oculta:** Intercepta el tráfico de red del mapa interactivo del SENAMHI usando `Selenium`.
* **Extracción Regex:** Localiza y decodifica la estructura JSON oculta (`var data = [...]`) dentro del código fuente.
* **Resultado:** Generación automática de un **Maestro de Estaciones** con +1900 puntos de medición georreferenciados.

### B. Algoritmo de Discriminación "Nieve vs. Lluvia"
Para evitar falsos positivos en zonas andinas, se aplica una lógica física sobre los datos crudos:

```python
# Lógica implementada en analisis_clima.py
Si (Precipitación > 0 mm):
    Si (Temperatura <= 2.0°C):
        Estado = "❄️ NIEVE/HELADA" (Justifica Cierre de Pista)
    Sino:
        Estado = "🌧️ LLUVIA LÍQUIDA" (Operación Estándar)

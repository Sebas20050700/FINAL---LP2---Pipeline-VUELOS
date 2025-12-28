# 📋 Requerimientos Técnicos - Etapa 1 (Ingesta)

Para garantizar la ejecución correcta del módulo de **Visual Crossing Engine** y la normalización geográfica del pipeline, el entorno debe cumplir con las siguientes especificaciones:

### 🐍 1. Entorno de Desarrollo
* **Lenguaje:** Python 3.8 o superior.
* **Entorno recomendado:** Google Colab (vínculo directo con GitHub) o VS Code con extensión Jupyter.

### 📦 2. Librerías de Python (Dependencias)
Es necesario instalar los siguientes paquetes para la gestión de datos y comunicación con el servidor:

| Librería | Propósito | Comando de Instalación |
| :--- | :--- | :--- |
| `requests` | Realizar peticiones HTTP a la API de Visual Crossing. | `pip install requests` |
| `pandas` | Estructuración de datos y generación del CSV maestro. | `pip install pandas` |
| `python-dotenv` | (Recomendado) Para la gestión segura de la API Key. | `pip install python-dotenv` |

### 🔑 3. Acceso y Credenciales
* **API Key:** Es obligatorio contar con una suscripción activa a **Visual Crossing Weather** mediante [RapidAPI](https://rapidapi.com/).
* **Conexión:** Acceso a internet habilitado para peticiones externas a `visual-crossing-weather.p.rapidapi.com`.
* **Cuota:** Disponibilidad de créditos en el plan de la API (Plan Free: 100 requests/día).

### 📂 4. Estructura de Salida
* El sistema requiere **permisos de escritura** en el directorio del proyecto para exportar el archivo `data_maestra_clima.csv` que servirá de insumo a los módulos de OpenSky y SENAMHI.
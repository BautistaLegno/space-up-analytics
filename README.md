# 🛰️ Space Up Analytics
**Prototipo de Soporte de Decisiones Agronómicas con Telemetría Satelital**
Desarrollado para el NASA Space Apps Challenge.

## 🚀 Sobre el Proyecto
Cuando hablamos de sequía agrícola, el daño más severo e irreversible ocurre a nivel microscópico: la pérdida de la **capacidad buffer** del suelo y la ruptura de los **enlaces amida** que sostienen el nitrógeno orgánico.

Space Up Analytics es un dashboard interactivo que traduce la telemetría de la misión satelital NASA SMAP (Soil Moisture Active Passive) en decisiones agronómicas y financieras. Al predecir el quiebre químico del suelo a través del estrés hídrico, la plataforma recomienda intervenciones biológicas tempranas (como la siembra de cultivos de servicio - Vicia villosa) que regeneran el suelo y evitan costosas remediaciones químicas.

💡 **Propuesta de Valor Económico:** Anticipar el colapso del suelo permite transformar una estrategia reactiva (remediación química de urgencia) en una preventiva (regeneración biológica natural), generando un **ahorro directo e inmediato de 90 USD por hectárea**.

## ⚙️ Características del Dashboard (MVP)
* **Geolocalización Dinámica:** Búsqueda por código postal o selección manual mediante clics en un mapa interactivo.
* **Monitoreo con NASA SMAP:** Simulación de humedad superficial cruzada con el índice de biomasa (NDVI) según la fenología del cultivo.
* **Sistema de Alerta Temprana:** Diagnóstico mediante semáforo de 3 estados (Óptimo, Precaución, Crítico) basado en la tolerancia buffer del lote.
* **Reporte Exportable:** Generación de un archivo `.txt` detallando la ubicación, el estado agronómico, la acción recomendada y el impacto económico.

## 🛠️ Tecnologías Utilizadas
* **Lenguaje:** Python 3
* **Entorno:** Google Colab / Jupyter Notebook
* **Librerías principales:** pandas, numpy, plotly, ipyleaflet, ipywidgets, geopy

## 📖 Cómo ejecutar el proyecto
1. **Preparar el Dataset:** Sube el archivo `Humedad_Suelo_SMAP_ZonaNucleo_2024.csv` a la raíz de tu Google Drive.
2. **Abrir el entorno:** Sube el archivo `.ipynb` de este repositorio a Google Colab.
3. **Ejecutar el Código:** Ve a `Entorno de ejecución > Ejecutar todas` y otorga permisos de lectura.
4. **Interacción:** Utiliza el panel izquierdo para buscar una ubicación o haz clic en el mapa. Ajusta los parámetros y ejecuta la simulación.

---
**Equipo Fundador:**
* 🌾 **Bautista Legno** - Lógica Agronómica y Negocios (Estudiante de Ingeniería Agronómica)
* 💻 **Thiago Osellame** - Arquitectura de Software y Datos (Estudiante de Ciencia de Datos)

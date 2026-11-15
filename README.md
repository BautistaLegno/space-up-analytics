# 🛰️ Space Up Analytics
**Sistema de Inteligencia Satelital y Soporte a la Decisión Agronómica**

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![Environment](https://img.shields.io/badge/Entorno-Google%20Colab%20%2F%20Jupyter-orange?logo=googlecolab&logoColor=white)
![Data Sources](https://img.shields.io/badge/APIs-NASA%20POWER%20%7C%20ORNL%20MODIS-green)
![Status](https://img.shields.io/badge/Estado-Operativo-brightgreen)

**Space Up Analytics** es una plataforma interactiva desarrollada en Python para entornos Jupyter / Google Colab. Combina telemetría satelital histórica, modelado estocástico de precipitaciones y reglas de decisión agronómica para asistir en la planificación de siembras y la auditoría técnica de lotes agrícolas (con foco inicial en la Zona Núcleo de Argentina).

---

## 📋 Tabla de Contenidos
1. [Características Principales](#-características-principales)
2. [Fuentes de Datos y Modelos](#-fuentes-de-datos-y-modelos)
3. [Estructura de la Interfaz](#-estructura-de-la-interfaz)
4. [Requisitos e Instalación](#-requisitos-e-instalación)
5. [Guía de Uso](#-guía-de-uso)
6. [Notas Técnicas y Respaldo](#-notas-técnicas-y-respaldo)

---

## 🚀 Características Principales

* **Geolocalización Interactiva:** Mapa satelital (`Esri.WorldImagery`) con delimitación visual de la Zona Núcleo argentina, buscador por localidad o código postal (vía *Nominatim*), selección por clic directo y registro de historial de búsquedas.
* **Análisis de Ventana de Siembra Actual:**
  * Evaluación de condiciones de germinación ($\pm 15$ días respecto a la fecha objetivo) considerando humedad superficial ($\text{cm}^3/\text{cm}^3$), temperatura de suelo ($^\circ\text{C}$) y ventanas fenológicas permitidas por cultivo.
  * **Recomendador Genético:** Sugerencia automatizada de longitud de ciclo (Corto, Intermedio, Largo o Híbrido Defensivo) basada en el agua útil del perfil (últimos 30 días), profundidad de napa y proyección climática a 45 días.
  * **Consulta Puntual:** Inspector diario para verificar valores reales o proyectados de humedad, temperatura de suelo y precipitaciones en cualquier fecha del rango.
* **Auditoría Técnica Histórica y Rotación:**
  * Seguimiento del vigor vegetativo mediante índice **NDVI real** (satélite MODIS, resolución $250\text{ m}$ cada 16 días) cruzado con curvas de humedad edáfica y precipitaciones.
  * Cálculo de balance estimado de Carbono aportado por rastrojos ($\text{t/ha}$), extracción teórica de macronutrientes ($\text{N-P-K}$) según secuencia de rotación de los últimos 5 años y cuantificación de días bajo estrés hídrico.
* **Exportación de Reportes:** Generación y descarga directa de un informe técnico en formato `.txt` (`Reporte_Auditoria_SpaceUp.txt`).

---

## 📡 Fuentes de Datos y Modelos

1. **NASA POWER API (`power.larc.nasa.gov`):**
   * Descarga series diarias desde el `01/01/2020` hasta 5 días previos a la fecha actual.
   * Parámetros utilizados: `GWETTOP` (humedad superficial convertida mediante porosidad promedio $\phi = 0.42$), `TS` (temperatura de suelo), `T2M` (temperatura del aire a $2\text{ m}$) y `PRECTOTCORR` (precipitación corregida).
2. **ORNL DAAC MODIS API (`modis.ornl.gov` - Producto `MOD13Q1`):**
   * Extrae mediciones reales de `250m_16_days_NDVI` para auditar campañas históricas, aplicando interpolación temporal entre pasadas satelitales.
3. **Motor de Proyección Climática (Cadenas de Markov):**
   * Cuando el ciclo evaluado supera la última fecha con datos satelitales reales, el sistema calcula las probabilidades de transición día seco/lluvioso ($P_{01}$ y $P_{11}$) propias del lote y simula la evolución diaria de lluvias, evaporación estacional y temperatura de suelo.

---

## 🖥️ Estructura de la Interfaz

La aplicación se divide en dos módulos principales dentro de un tablero unificado de `ipywidgets`:

* **Sección 1: Planificación de Siembra Actual**
  * *Panel Izquierdo:* Buscador de localidades, mapa interactivo y buscador puntual por fecha.
  * *Panel Derecho:* Selector de cultivo (Trigo, Cebada, Maíz Temprano/Tardío, Girasol, Soja 1ra/2da, Sorgo, Pasturas), fecha tentativa, profundidad de napa freática ($\text{m}$) y umbral crítico de humedad (*Buffer*).
  * *Salida:* Tarjetas KPI, recomendación genética y gráfico interactivo *Plotly* de doble eje (germinación $\pm 15$ días).
* **Sección 2: Auditoría Técnica Histórica**
  * *Controles:* Selección de campaña (2021–2025), fechas reales de siembra/cosecha y matriz de rotación de los 5 años previos.
  * *Salida:* Panel resumen de impacto nutricional/hídrico, gráfico histórico macro de humedad vs. NDVI satelital y botón de descarga de reporte.

---

## ⚙️ Requisitos e Instalación

El código está diseñado para ejecutarse directamente en **Google Colab** (donde `google.colab.files` está disponible de forma nativa). Si deseas ejecutarlo en un entorno local de Jupyter Notebook/Lab, asegúrate de instalar las siguientes dependencias:

```bash
pip install ipywidgets pandas numpy plotly ipyleaflet geopy requests
```

*(Nota: En entornos locales fuera de Google Colab, reemplaza la función `files.download()` en `exportar_datos` por un guardado local estándar).*

---

## 📖 Guía de Uso

1. **Ubicar el Lote:** Ingresa una ciudad o código postal en el buscador (ej. `Cruz Alta`) o haz clic directamente sobre el lote en el mapa satelital.
2. **Configurar la Siembra:** Selecciona el cultivo a implantar, la fecha deseada, la profundidad de napa y ajusta el umbral mínimo de humedad (*Buffer*).
3. **Ejecutar Análisis:** Haz clic en **`Descargar API y Analizar Siembra`**. Espera unos segundos mientras se consultan los servidores de NASA POWER y observa el diagnóstico de ventana y recomendación de ciclo.
4. **Auditar Campañas Pasadas:** Desplázate hacia la **Sección 2**, verifica la secuencia de rotación de los últimos 5 años, elige la campaña a auditar y presiona **`Evaluar Historial`**.
5. **Descargar Informe:** Una vez generada la auditoría histórica, presiona **`Descargar Reporte`** para obtener el resumen técnico en tu dispositivo.

---

## 🛡️ Notas Técnicas y Respaldo

* **Modo Offline / Respaldo Automático:** Si las APIs externas (NASA POWER o MODIS) experimentan demoras o caídas de red, el sistema activa automáticamente un **Simulador Climático Estacional** y curvas **NDVI Bioclimáticas** ajustadas por estrés hídrico para garantizar la continuidad operativa de la interfaz.
* **Caché de Telemetría:** Las consultas a MODIS se almacenan en memoria (`cache_ndvi_modis`) por coordenadas y rango de fechas para acelerar re-evaluaciones sobre el mismo lote.
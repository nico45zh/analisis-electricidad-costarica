# Análisis de precios medios de electricidad en Costa Rica (2025–2026)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nico45zh/analisis-electricidad-costarica/blob/main/notebooks/data_analytics.ipynb)

Análisis exploratorio y predictivo de los precios medios de la electricidad de los cuatro principales operadores públicos de distribución de Costa Rica (**ICE, CNFL, ESPH y JASEC**) entre **enero de 2025 y agosto de 2026**, a partir de los datos abiertos de la Autoridad Reguladora de los Servicios Públicos (ARESEP).

El repositorio contiene el notebook de Python con todo el flujo de análisis, los datos de origen y el artículo en formato IEEE que reporta los resultados.

## Principales hallazgos

- En 2026 el precio medio bajó hasta un 13,9 % respecto de 2025, sin que disminuyeran las ventas.
- Los precios no siguen una tendencia lineal, sino un cambio de nivel en febrero de 2026, explicado porque el Costo Variable de Generación (CVG) pasó de recargo a descuento, y no por una baja de la tarifa base.
- El orden de precios ICE > CNFL > ESPH > JASEC se mantiene en los 20 meses del periodo.
- Ningún modelo (regresión lineal ni ARIMAX) superó al pronóstico ingenuo en la prueba fuera de muestra (mayo a agosto de 2026), y todos tuvieron errores menores al 2 %.

## Estructura del repositorio

```
analisis-electricidad-costarica/
├── datos/
│   ├── Datos_Abiertos_ARESEP_Precios_medios_2025.csv   # enero a diciembre de 2025
│   └── Datos_Abiertos_ARESEP_Precios_medios_2026.csv   # enero a agosto de 2026
├── notebooks/
│   └── data_analytics.ipynb                            # flujo completo del análisis
└── latex/
    ├── main.tex                                        # artículo (IEEEtran, conferencia)
    ├── boiboites.sty
    ├── tablas/                                         # cuadros generados por el notebook
    └── figuras/                                        # figuras 01 a 07 del artículo
```

## Contenido del notebook

| Sección | Contenido |
|---|---|
| 1. Carga de datos | Descarga de 2025 y 2026 desde la API de la ARESEP; si no responde, usa los CSV de `datos/` publicados en este repositorio. Une ambos años y guarda un respaldo sin transformar. |
| 2. Limpieza | Normalización de texto, filtro de los cuatro operadores, columna `fecha`, revisión de calidad, eliminación de registros con `ventas <= 0` y recálculo del precio medio. |
| 3. Análisis exploratorio | Estadísticas descriptivas, precio medio ponderado mensual, comparación de trimestres equivalentes, efecto del CVG y ranking de operadores. |
| 4. Regresión y pronóstico | Regresión lineal simple sobre el tiempo, regresión con variable indicadora para 2026 (errores Newey-West) y evaluación del pronóstico frente al modelo ingenuo y un ARIMAX, con escenario a diciembre de 2026. |
| 5. Material para el artículo | Generación de los cuadros en LaTeX y del inventario de figuras. |

## Cómo ejecutarlo

**En Google Colab (recomendado):** abra el notebook con el botón *Open in Colab* de arriba y ejecute todas las celdas. No requiere configuración adicional.

**En local:**

```bash
git clone https://github.com/nico45zh/analisis-electricidad-costarica.git
cd analisis-electricidad-costarica
pip install pandas numpy matplotlib scipy statsmodels requests jupyter
jupyter notebook notebooks/data_analytics.ipynb
```

Al ejecutarse, el notebook crea en su carpeta de trabajo:

- `data/precios_medios_raw.csv` y `data/precios_medios_limpio.csv`: datos crudos y limpios.
- `figuras/`: las figuras del artículo en PNG (300 dpi).
- `tablas/`: los cuadros en formato LaTeX.

Las figuras y cuadros de `latex/` son copias de esos resultados.

## Compilar el artículo

El artículo usa la clase `IEEEtran` y se compila con `pdflatex` desde la carpeta `latex/` (también puede subirse la carpeta completa a Overleaf):

```bash
cd latex
pdflatex main.tex
pdflatex main.tex
```

## Fuente de datos

ARESEP, Datos Abiertos: *Tarifas de electricidad, precios medios*. Servicio web:
`https://datos.aresep.go.cr/ws.datosabiertos/Services/IE/TarifasElectricidad.svc/ObtenerTarifasPreciosMedios/{año}`

El estudio se limita a los cuatro operadores públicos de distribución; las cooperativas de electrificación rural quedan fuera del alcance, y su inclusión se plantea como trabajo futuro.

## Autores

Escuela de Informática, Universidad Nacional de Costa Rica (UNA), Heredia.

Estudiantes:
- Ana Lucía Salazar Chacón
- Nicolas Zárate Hernández
- Gerson Jafet Madrigal Cubillo
- Sandy Marchena Blanco


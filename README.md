# Experimento A/B en landing page: ¿qué versión convierte más y genera más valor?

> Análisis estadístico de un experimento A/B sobre dos versiones de una página de inicio para recomendar cuál implementar, con base en conversión, gasto y segmentación de usuarios.

## Resumen

La **versión B** supera a la versión A en las dos métricas principales:

| Métrica | Página A | Página B | Diferencia |
|---|---|---|---|
| Tasa de conversión | 12.57% | **15.96%** | +3.38 puntos porcentuales |
| Gasto promedio por usuario convertido | $61.09 | **$68.75** | +$7.66 |

Ambas diferencias son estadísticamente significativas (α = 0.05). **Recomendación: implementar la página B como versión definitiva.**

**Summary (English):** Version B of the landing page converts better than version A (15.96% vs 12.57%) and its converted users spend more on average ($68.75 vs $61.09). Both differences are statistically significant (Z-test and Welch's t-test), so version B is recommended. Traffic source shows a modest association with conversion; user type (new vs. returning) does not.

## Pregunta de negocio

Una empresa probó dos versiones de su página de inicio con tráfico real y necesita decidir cuál implementar. El análisis responde:

1. ¿Qué versión genera una mayor tasa de conversión?
2. ¿Qué versión genera un mayor gasto por cliente?
3. ¿Qué fuentes de tráfico se asocian con más conversión?
4. ¿Influye el tipo de usuario (nuevo o recurrente) en la conversión?

## Datos

Archivo `landing_experiment.csv`: **40,000 usuarios** expuestos al experimento entre el **1 y el 28 de enero de 2026**, repartidos de forma equilibrada (A: 19,982 · B: 20,018). Sin valores nulos ni duplicados (cada `user_id` es único).

| Columna | Descripción |
|---|---|
| `user_id` | Identificador único del usuario |
| `date` | Fecha de exposición a la página |
| `landing` | Versión mostrada (A o B) |
| `region` | Región del usuario (Norte, Centro, Sur, Occidente, Oriente) |
| `dispositivo` | Mobile o Desktop |
| `traffic_source` | Canal de llegada (Organic, Ads, Email, Referral) |
| `user_type` | Nuevo o Recurrente (según historial previo) |
| `converted` | 1 si el usuario convirtió, 0 si no |
| `gasto` | Monto gastado (0 si no convirtió) |

> Fuente: [completa aquí si es un dataset del bootcamp, simulado o de otra procedencia].

## Metodología

1. **Carga y validación:** revisión de tipos, nulos, duplicados y categorías; conversión de `date` a formato fecha.
2. **Gasto por usuario convertido:** prueba de Levene (igualdad de varianzas) y **prueba t de Welch**.
3. **Tasa de conversión:** **prueba Z de proporciones** entre A y B.
4. **Fuente de tráfico vs. conversión:** **chi-cuadrado de independencia**.
5. **Tipo de usuario vs. conversión:** **chi-cuadrado de independencia**.
6. **Visualización:** gráficas de conteos y proporciones que respaldan cada prueba.
7. **Insight ejecutivo:** conclusiones y recomendaciones para el negocio.

Nivel de significancia en todas las pruebas: α = 0.05.

## Resultados

| Análisis | Prueba | Resultado | Decisión |
|---|---|---|---|
| Conversión A vs B | Z de proporciones | Z = −9.68 · p ≈ 3.8×10⁻²² | Diferencia significativa |
| Gasto por convertido A vs B | Welch (tras Levene, p ≈ 6.9×10⁻⁸) | t = −9.48 · p ≈ 3.6×10⁻²¹ | Diferencia significativa |
| Fuente de tráfico vs conversión | Chi-cuadrado (gl = 3) | χ² = 8.66 · p = 0.034 | Asociación modesta |
| Tipo de usuario vs conversión | Chi-cuadrado (gl = 1) | χ² = 0.51 · p = 0.47 | Sin asociación significativa |

**Hallazgos principales**

- **Página B:** 3,194 de 20,018 usuarios convirtieron (15.96%) frente a 2,512 de 19,982 en A (12.57%). Además, los usuarios convertidos de B gastan en promedio $7.66 más.
- **Fuente de tráfico:** Organic aporta más conversiones en volumen (2,480) porque es la que más tráfico trae (17,987 usuarios). En tasa, **Email (14.99%) y Ads (14.74%)** superan ligeramente a Referral (13.88%) y Organic (13.79%). La diferencia es estadísticamente significativa pero pequeña.
- **Tipo de usuario:** las tasas son prácticamente iguales (Nuevo 14.36% · Recurrente 14.09%); no hay evidencia de que influya en la conversión.

<!-- Agrega aquí 2 o 3 gráficas del notebook guardadas en /images, por ejemplo:
![Tasa de conversión por página](images/conversion_ab.png)
![Gasto promedio por usuario convertido](images/gasto_ab.png)
-->

## Recomendaciones

- **Implementar la página B**: convierte más y sus clientes gastan más.
- **Evaluar los canales Email y Ads** como candidatos para reforzar, validando primero su costo de adquisición, ya que el análisis no incluye datos de costo.
- Mantener el canal Organic por su volumen.

## Limitaciones y próximos pasos

- **Duración:** el experimento cubre 28 días; podría haber efecto de novedad o estacionalidad.
- **Sin datos de costo ni margen:** la recomendación se basa en conversión y gasto, no en rentabilidad.
- **Gasto por usuario convertido:** el gasto se compara solo entre quienes convirtieron. Como B cambia también *quién* convierte, un siguiente paso es comparar el **ingreso por usuario expuesto** (incluyendo a quienes no compraron).
- **Tamaño del efecto:** con 40,000 usuarios casi cualquier diferencia resulta significativa; falta reportar intervalos de confianza de la diferencia.
- **Validez del experimento:** falta verificar formalmente el balance de región, dispositivo, fuente y fechas entre A y B.
- **Segmentos:** analizar si el efecto de B cambia según tipo de usuario, dispositivo o región (interacciones).

## Estructura del repositorio

```
├── README.md
├── S9_Version_Student_Proyecto_Landing_Experiment.ipynb   # análisis completo
└── landing_experiment.csv                                  # datos (si puedes redistribuirlos)
```

## Cómo reproducirlo

1. Clona o descarga este repositorio.
2. Asegúrate de tener `landing_experiment.csv`. El notebook lo lee de `/datasets/landing_experiment.csv` (ruta del entorno del bootcamp); ajusta la ruta de la celda de carga a donde guardes el archivo.
3. Instala las dependencias:

```bash
pip install pandas seaborn matplotlib scipy statsmodels
```

4. Abre el notebook y ejecuta las celdas en orden:

```bash
jupyter notebook S9_Version_Student_Proyecto_Landing_Experiment.ipynb
```

## Herramientas

Python 3 · pandas · seaborn · matplotlib · scipy · statsmodels · Jupyter Notebook

## Autor

**[Carlos Carrillo Aguayo]** · Data Analyst  
[LinkedIn](https://www.linkedin.com/in/carlosagca-data/) · [Más proyectos](https://github.com/carlosagca93-ui)

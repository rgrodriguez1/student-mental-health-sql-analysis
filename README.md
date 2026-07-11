# 🧠 Análisis SQL: Salud mental de estudiantes internacionales

## Contexto

¿Ir a la universidad en otro país afecta la salud mental? Este proyecto explora datos de una encuesta
universitaria japonesa (2018) a estudiantes internacionales y nacionales, para ver si la duración de la
estancia en el país está relacionada con indicadores de salud mental: depresión, conexión social y estrés
de aculturación.

## Fuente de datos

Dataset académico `students`, usado como ejercicio de práctica en DataCamp. Columnas relevantes:

| Columna   | Descripción                                        |
|-----------|-----------------------------------------------------|
| inter_dom | Tipo de estudiante (internacional o nacional)        |
| stay      | Tiempo de estancia actual (años)                     |
| todep     | Puntuación de depresión (test PHQ-9)                 |
| tosc      | Puntuación de conexión social (test SCS)             |
| toas      | Puntuación de estrés de aculturación (test ASISS)    |

## Metodología

Usando PostgreSQL, filtré solo estudiantes internacionales y agrupé por duración de estancia, calculando
el promedio de cada puntuación de salud mental:

```sql
SELECT stay,
       COUNT(*) AS count_int,
       ROUND(AVG(todep)::numeric, 2) AS average_phq,
       ROUND(AVG(tosc)::numeric, 2) AS average_scs,
       ROUND(AVG(toas)::numeric, 2) AS average_as
FROM students
WHERE inter_dom = 'Inter' AND stay IS NOT NULL
GROUP BY stay
ORDER BY stay DESC;
```

## Resultados

| stay | count_int | average_phq | average_scs | average_as |
|------|-----------|--------------|--------------|------------|
| 10   | 1         | 13.00        | 32.00        | 50.00      |
| 8    | 1         | 10.00        | 44.00        | 65.00      |
| 7    | 1         | 4.00         | 48.00        | 45.00      |
| 6    | 3         | 6.00         | 38.00        | 58.67      |
| 5    | 1         | 0.00         | 34.00        | 91.00      |
| 4    | 14        | 8.57         | 33.93        | 87.71      |
| 3    | 46        | 9.09         | 37.13        | 78.00      |
| 2    | 39        | 8.28         | 37.08        | 77.67      |
| 1    | 95        | 7.48         | 38.11        | 72.80      |

*(average_phq: puntuación de depresión · average_scs: conexión social · average_as: estrés de aculturación)*

## Conclusión

Para los grupos con muestra suficiente (estancias de 1 a 4 años, que concentran la mayoría del
estudiantado internacional encuestado), se observa que a mayor duración de la estancia, la puntuación de
conexión social tiende a bajar y el estrés de aculturación tiende a subir — un patrón inverso al que uno
esperaría si la adaptación mejorara con el tiempo. Esto es consistente con la idea del estudio original de
que la conexión social y el estrés de aculturación están relacionados con la salud mental del estudiantado
internacional, aunque en este caso la duración de la estancia no aparece como un factor protector claro.

Los grupos con estancias más largas (5 a 10 años) tienen tamaños de muestra muy pequeños (n=1 a 3), por lo
que sus promedios deben interpretarse con cautela y no permiten sacar conclusiones sólidas sobre ese
segmento.

## Herramientas

SQL (PostgreSQL) · DataCamp DataLab

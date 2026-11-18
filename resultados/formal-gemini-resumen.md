# Comparación formal

## Resultados de la réplica

| Métrica | Referencia histórica | Mediana | Rango |
| :---- | ----: | ----: | ----: |
| TP | 17.000 | 20.000 | 20.000 a 20.000 |
| FP | 3.000 | 0.000 | 0.000 a 0.000 |
| FN | 3.000 | 0.000 | 0.000 a 0.000 |
| duplicado | N/A | 0.000 | 0.000 a 0.000 |
| precision | 0.850 | 1.000 | 1.000 a 1.000 |
| recall | 0.850 | 1.000 | 1.000 a 1.000 |
| f1 | 0.850 | 1.000 | 1.000 a 1.000 |
| fraccion\_alertas\_incorrectas | N/A | 0.000 | 0.000 a 0.000 |

Latencia media por corrida

| Corrida | Recuperación s | Detector s | Informe s | Total comparable s |
| :---- | ----: | ----: | ----: | ----: |
| Paper | 2.07 | 70.00 | 13.00 | 85.10 |
| formal-gemini-r1 | 1.26 | 9.39 | 4.91 | 15.56 |
| formal-gemini-r2 | 1.26 | 9.97 | 3.83 | 15.06 |
| formal-gemini-r3 | 1.12 | 10.37 | 4.58 | 16.06 |

La comparación es histórica. El estudio usa plantillas sustitutas, Gemini 3.8 flash y un índice NumPy.

[Tablas 1 y 2 del artículo](https://ieeexplore.ieee.org/document/10965635)
flowchart LR
    subgraph F0["FASE 0 · Corpus, referencia y fuentes"]
        direction TB
        subgraph Corpus["Corpus formal · 10 plantillas AWS CloudFormation"]
            I1["<b>T01–T05 · Casos con incumplimientos</b><br/>20 incumplimientos de referencia"]
            I2["<b>T06–T10 · Casos de control</b><br/>Sin incumplimientos en los controles evaluados"]
            GT["<b>Referencia de evaluación</b><br/>referencia.csv revisado y cerrado antes de la corrida"]
        end
        subgraph FuentesKB["Fuentes de conocimiento"]
            K1["<b>Documentación oficial de AWS</b><br/>11 fragmentos en la base formal"]
            K2["<b>Aviso de AWS Secrets Manager</b><br/>Insumo para la prueba documental complementaria"]
        end
    end

    subgraph F1["FASE 1 · Ingesta y vectorización"]
        direction TB
        N1["<b>Gemini Embedding 2</b><br/>Vectores de 768 dimensiones"]
        N2[("<b>Índice vectorial con NumPy</b><br/>Persistido en Google Drive")]
    end

    subgraph F2["FASE 2 · Preprocesamiento y recuperación contextual"]
        direction TB
        N3["<b>Parser y normalizador de CloudFormation</b><br/>Python · YAML/JSON · recursos, condiciones y referencias"]
        N4["<b>Recuperación RAG por similitud coseno</b><br/>Top 3 por tipo de recurso · máximo 12 fragmentos por plantilla"]
    end

    subgraph F3["FASE 3 · Detección y generación del informe en Colab"]
        direction TB
        N5["<b>Ensamblaje del prompt</b><br/>Plantilla original, resumen, contexto, controles y fragmentos"]
        N6["<b>Detector · Gemini 3.8 Flash</b><br/>Hallazgos, evidencia y acciones sugeridas en JSON"]
        N7["<b>Generador de informes · Gemini 3.8 Flash</b><br/>Impacto, prioridad y recomendaciones en Markdown"]
    end

    subgraph F4["FASE 4 · Evaluación formal y resultados"]
        direction TB
        N8["<b>Evaluador en Python</b><br/>Emparejamiento por plantilla, recurso y control<br/>3 repeticiones de 10 plantillas"]
        subgraph Resultados["Resultados de la réplica"]
            R1["<b>Detección por repetición</b><br/>20 aciertos · 0 falsos positivos · 0 omisiones"]
            R2["<b>Precisión, recall y F1 del 100 %</b><br/>Mismo resultado en las 3 repeticiones"]
            R3["<b>15,56 s de media por plantilla</b><br/>Recuperación, detección e informe"]
        end
        subgraph Complementarias["Pruebas complementarias"]
            C1["Revisión cualitativa de las recomendaciones"]
            C2["Comparación antes y después de actualizar la base documental"]
        end
    end

    I1 --> GT
    I2 --> GT
    I1 -->|Plantillas YAML/JSON y contexto| N3
    I2 -->|Plantillas YAML/JSON y contexto| N3
    K1 -->|Fragmentos documentales| N1
    N1 -->|Vectores| N2
    N3 -->|Descriptores de recursos| N4
    N4 -->|Consulta vectorial| N2
    N2 -->|Fragmentos más similares| N4
    N3 -->|Plantilla original y resumen normalizado| N5
    N4 -->|Fragmentos recuperados| N5
    N5 -->|Prompt estructurado| N6
    N6 -->|Hallazgos y acciones sugeridas| N7
    N6 -->|hallazgos.json| N8
    N7 -->|informe.md| N8
    GT -->|Etiquetas de referencia| N8
    N8 --> R1
    N8 --> R2
    N8 --> R3
    N7 -.-> C1
    N2 -.-> C2
    K2 -.-> C2
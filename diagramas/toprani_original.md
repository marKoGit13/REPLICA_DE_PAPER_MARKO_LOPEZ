flowchart TB
    %% ==========================================
    %% FASE 0: PRERREQUISITOS, INPUTS Y "COMIDA"
    %% ==========================================
    subgraph F0 ["FASE 0: Estado Inicial, Prerrequisitos e Inputs"]
        direction TB
        
        subgraph Corpus ["1. Corpus Experimental (10 Plantillas IaC)"]
            I1["<b>T1 - T5: Plantillas Vulnerables</b>"]
            I2["<b>T6 - T10: Plantillas Seguras (Control)</b>"]
            GT["<b>N0_GT: Anotación Ground-Truth</b>"]
        end

        subgraph FuentesKB ["2. Fuentes de Conocimiento de Seguridad"]
            K1["<b>Documentación Normativa (AWS / CIS)</b>"]
            K2["<b>Actualizaciones Dinámicas (Advisories)</b>"]
        end
    end

    %% ==========================================
    %% FASE 1: PREPARACIÓN DE LA BASE RAG
    %% ==========================================
    subgraph F1 ["FASE 1: Ingesta y Vectorización (Setup RAG)"]
        direction TB
        N1["<b>N1: Motor de Embeddings</b><br/><i>Titan Text Embeddings V2</i>"]
        N2[("<b>N2: Vector Database / Knowledge Base</b><br/><i>Amazon OpenSearch</i>")]
    end

    %% ==========================================
    %% FASE 2: PREPROCESAMIENTO Y RECUPERACIÓN
    %% ==========================================
    subgraph F2 ["FASE 2: Preprocesamiento y Recuperación Contextual"]
        direction TB
        N3["<b>N3: Parser y Normalizador CloudFormation</b>"]
        N4["<b>N4: Agente de Recuperación (RAG Retrieval)</b>"]
    end

    %% ==========================================
    %% FASE 3: ORQUESTACIÓN MULTI-AGENTE LLM
    %% ==========================================
    subgraph F3 ["FASE 3: Orquestación Multi-Agente (Amazon Bedrock)"]
        direction TB
        N5["<b>N5: Ensamblador de Prompt</b>"]
        N6["<b>N6: Vulnerability Detection Agent</b><br/><i>Claude 3.5 Sonnet V2</i>"]
        N7["<b>N7: Report-Generation Agent</b><br/><i>Claude 3.5 Sonnet V2</i>"]
    end

    %% ==========================================
    %% FASE 4: EVALUACIÓN Y RESULTADOS
    %% ==========================================
    subgraph F4 ["FASE 4: Validación Experimental y Resultados"]
        direction TB
        N8["<b>N8: Evaluación vs. Ground-Truth y Casos Borde</b>"]
        
        subgraph Resultados ["Métricas Finales"]
            R1["<b>Detección: 85% Recall / Precision / F1</b>"]
            R2["<b>Falsos Positivos (~15%) y Sobre-remediación (~5%)</b>"]
            R3["<b>Rendimiento CI/CD (85.1 s promedio)</b>"]
        end
    end

    %% ==========================================
    %% RELACIONES Y FLUJO DE DATOS
    %% ==========================================
    I1 & I2 --> GT
    I1 & I2 -->|"Plantilla YAML/JSON"| N3
    K1 & K2 -->|"Documentos crudos"| N1
    N1 -->|"Vectores indexados"| N2
    
    N3 -->|"Descriptores de recursos"| N4
    N2 <-->|"Query vectorial / Top-k Snippets"| N4
    
    N3 -->|"Resumen normalizado + Raw IaC"| N5
    N4 -->|"Snippets relevantes"| N5
    
    N5 -->|"Prompt estructurado"| N6
    N6 -->|"Hallazgos + justificación + fix"| N7
    
    N7 -->|"Reporte Markdown"| N8
    GT -->|"Etiquetas reales"| N8
    
    N8 --> R1 & R2 & R3
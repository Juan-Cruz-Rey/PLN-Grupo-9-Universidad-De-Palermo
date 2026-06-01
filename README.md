# Proyecto NER — Procesamiento del Lenguaje Natural
**Universidad de Palermo — Grupo 9**

## Descripción

Sistema de **Reconocimiento de Entidades Nombradas (NER)** aplicado a la gestión de incidentes en aulas. El objetivo es extraer automáticamente información estructurada a partir de mensajes en lenguaje natural enviados por docentes (ej: *"no tengo marcadores en el aula 4"*), permitiendo la generación automática de tickets de soporte sin intervención manual.

## Entidades

| Etiqueta | Descripción | Ejemplo |
|---|---|---|
| `OBJETO` | Elemento físico afectado | *"marcador"*, *"proyector"*, *"portero eléctrico"* |
| `PROBLEMA` | Falla o incidente reportado | *"no funciona"*, *"está roto"*, *"pierde agua"* |
| `LUGAR` | Ubicación del incidente | *"aula 04-05"*, *"biblioteca"*, *"laboratorio"* |

## Dataset

- **570 ejemplos** anotados manualmente y aumentados con herramientas de IA
- Formato JSONL con spans de caracteres y etiquetas
- División: 80% entrenamiento / 10% desarrollo / 10% validación

## Modelos

Se entrenaron y compararon tres enfoques:

1. **spaCy fine-tuned (`es_core_news_lg`)** — modelo grande de español con embeddings preentrenados
2. **CRF (Conditional Random Field)** — features artesanales con esquema BIO (`sklearn-crfsuite`)
3. **spaCy fine-tuned (`es_core_news_sm`)** — modelo liviano de español

## Resultados

#### Coincidencia exacta (validación)

| Modelo | Precisión | Recall | F1 |
|---|---|---|---|
| spaCy lg | 0.737 | 0.727 | 0.732 |
| CRF | **0.760** | 0.740 | **0.750** |
| spaCy sm | 0.750 | 0.740 | 0.745 |

#### Coincidencia parcial (umbral 70%)

| Modelo | Precisión | Recall | F1 |
|---|---|---|---|
| spaCy lg | 0.831 | 0.820 | 0.826 |
| CRF | 0.829 | 0.807 | 0.818 |
| spaCy sm | **0.845** | **0.833** | **0.839** |

> El modelo CRF obtiene el mejor F1 en coincidencia exacta, mientras que spaCy sm supera a los demás en coincidencia parcial. La entidad más difícil es `PROBLEMA` por su alta variabilidad léxica; `LUGAR` es la más fácil por sus patrones estructurados.

## Estructura del repositorio

```
├── Notebook - Proyecto NER.ipynb   # Notebook principal con todo el pipeline
├── dataset_limpio.jsonl            # Dataset anotado
├── config/                         # Archivos de configuración de spaCy
└── data/                           # Datos en formato binario spaCy (.spacy)
```

## Tecnologías

`spaCy` · `sklearn-crfsuite` · `srsly` · `scikit-learn` · `matplotlib` · `seaborn`

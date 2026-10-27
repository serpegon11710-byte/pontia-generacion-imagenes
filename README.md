# Fine-Tuning de Stable Diffusion v1.4 con LoRA: Estilo de Ilustraciones de Libros Antiguos

Este proyecto implementa el ajuste fino (*fine-tuning*) del modelo de generación de imágenes **Stable Diffusion v1.4** utilizando la técnica de adaptación de bajo rango **LoRA (Low-Rank Adaptation)**. El objetivo principal es adaptar el modelo base para especializar su capacidad de síntesis en el estilo visual característico de las ilustraciones históricas.

El entrenamiento y la evaluación del flujo completo se han desarrollado en un cuaderno interactivo diseñado para ejecutarse sobre **Google Colab**, aprovechando los recursos de aceleración por hardware (GPU) necesarios para el procesamiento de tensores e inferencia con modelos de difusión.

---

## 📸 Artefactos y Repositorio de Pesos

Los adaptadores LoRA obtenidos tras el entrenamiento han sido publicados de forma pública en Hugging Face Hub y pueden ser importados directamente mediante la librería `diffusers`:

* **Repositorio en Hugging Face:** [`serpegon/stable-diffusion-v1-4-oldbookillustrations-lora`](https://huggingface.co/serpegon/stable-diffusion-v1-4-oldbookillustrations-lora)

---

## 🎯 Objetivos del Proyecto

1. **Adaptación de Estilo:** Entrenar adaptadores de bajo rango que capturen la textura, el trazo y los tonos estéticos de grabados e ilustraciones de libros antiguos.
2. **Eficiencia en Recursos:** Emplear la metodología **LoRA** sobre los componentes del modelo UNet para reducir drásticamente el uso de memoria VRAM y el número de parámetros entrenables respecto a un fine-tuning completo (*Full Fine-Tuning*).
3. **Flujo Integrado en la Nube:** Ejecutar el pipeline de extracción, preprocesamiento, ajuste e inferencia comparativa dentro del entorno de Google Colab.

---

## 🗄️ Dataset Utilizado

* **Dataset:** [`gigant/oldbookillustrations`](https://huggingface.co/datasets/gigant/oldbookillustrations) (Licencia CC BY-NC 4.0).
* **Campos Seleccionados:**
  * **Imágenes:** Columna de resolución ultra alta (`1600px`).
  * **Textos:** Columna de descripciones históricas y alt-text (`info_alt`).
* **Tratamiento de Datos:**
  * Redimensionado y recorte al centro (*Center Crop*) estandarizado a **512×512 píxeles**.
  * Imputación de descripciones vacías o ausentes mediante etiquetas genéricas para evitar descarte innecesario de muestras.

---

## ⚙️ Entorno de Ejecución y Tecnologías

El notebook está preparado para ejecutarse de forma directa en **Google Colab** utilizando un entorno con aceleración GPU (T4 o superior).

* **Framework Base:** PyTorch
* **Librerías Clave:** `diffusers`, `transformers`, `accelerate`, `peft`, `datasets`, `safetensors`.
* **Optimización:** Batch size unitario ($1$) con acumulación de gradiente ($4$ pasos) para optimizar el consumo de VRAM en sesiones gratuitas/estándar.

---

## 🛠️ Resoluciones e Incidencias en Entrenamiento

Durante la fase de preparación e integración técnica dentro del entorno Colab se gestionaron dos incidencias críticas:

1. **Conflicto de Dependencias:** Se resolvió una incompatibilidad entre versiones específicas de `torchao` y `peft` eliminando el paquete conflictivo antes del inicio del entrenamiento.
2. **Robustez del Pipeline de Datos:** Se incorporó un control de excepciones para capturar e ignorar imágenes dañadas o truncadas en el dataset original, garantizando que la ejecución del bucle de entrenamiento continuara sin interrupciones.

---

## 🧪 Evaluación y Comparativa Lado a Lado (*Side-by-Side*)

El proyecto incluye un pipeline de validación que contrasta los resultados generados por el modelo base `CompVis/stable-diffusion-v1-4` frente a la versión con los adaptadores LoRA integrados.

* **Condiciones de Inferencia:** Se fijan idénticas semillas aleatorias (*seeds*), número de pasos de inferencia y descripciones (*prompts*) para aislar únicamente el impacto del aprendizaje de estilo.
* **Composición:** El notebook genera automáticamente imágenes compuestas de comparación directa (Modelo Base vs. Modelo con LoRA) para facilitar el análisis cualitativo.

---

## 📋 Estructura del Notebook

1. **Preparar el entorno:** Inicialización de GPU, instalación de dependencias y verificación de hardware.
2. **Carga y exploración del dataset:** Descarga del corpus `gigant/oldbookillustrations` e inspección de estructura.
3. **Preprocesamiento y validación de imágenes:** Adaptación de resolución a 512×512 y tratamiento de metadatos.
4. **Configuración del repositorio en Hugging Face:** Autenticación y preparación de la estructura remota para exportación.
5. **Ejecución del entrenamiento mediante LoRA:** Llamada al script de entrenamiento optimizado y gestión de incidencias.
6. **Verificación y validación de los artefactos guardados:** Confirmación de integridad de los archivos `.safetensors`.
7. **Evaluación comparativa:** Generación de imágenes bajo condiciones controladas con y sin adaptadores LoRA.
8. **Composición comparativa y exportación:** Creación de composiciones *side-by-side* y descarga de resultados.
9. **Checklist de entrega y verificación:** Resumen final del estado del proyecto y enlaces a artefactos.
Taller práctico 1 

# Integrantes:
- Joshua Triana
- Andres Borrera

# Fase 1: Selección y Justificación del Modelo de IA para EcoMarket

### 1. ¿Qué tipo de modelo de IA generativa es el más adecuado?

El modelo más adecuado es el GPT-4 ajustado con datos de EcoMarket

### 2. ¿Por qué este modelo y no otro?

Se elige GPT-4 porque:

- **Precisión en respuestas específicas:** GPT-4 puede manejar consultas complejas y brindar respuestas precisas cuando está afinado con datos específicos de la empresa, como el estado del pedido y políticas de devoluciones.

- **Fluidez y naturalidad:** GPT-4 genera respuestas naturales, empáticas y coherentes, mejorando la experiencia del usuario en interacciones tanto simples como complejas.

- **Escalabilidad y adaptabilidad:** Puede ajustarse mediante fine-tuning o mediante prompts especializados, permitiendo ampliar o disminuir su alcance según necesidades.

- **Versatilidad:** Es capaz de atender tanto consultas repetitivas como temas más específicos que requieren mayor comprensión contextual.

**Comparación con otros modelos:**

- Modelos pequeños o específicos (BERT, DistilBERT, etc): Son más eficientes en tareas concretas y suelen ser menos costosos, pero no generan respuestas en lenguaje natural tan fluido como GPT-4.

- Modelos anteriores como GPT-3.5: GPT-4 presenta mejoras en comprensión, generación de texto más coherente y manejo de instrucciones complejas.

- Modelos posteriores como GPT-5: Utilizar esta versión en la fase primitiva en la que se encuentra sería riesgoso, ya que aún le falta maduración.

- Otros modelos (Gemini, DeepSeek, etc): Han sido menos probados en el entorno empresarial cuando se compara con GPT.

### 3. ¿Cuál sería la arquitectura propuesta?

La arquitectura propuesta sería:

- Modelo base: GPT-4, que se complementaría con un proceso de fine-tuning usando datos específicos de EcoMarket.

- Integración con bases de datos: El modelo se conectaría a una API que extracte información en tiempo real desde la base de datos de EcoMarket, como estados de pedido y detalles de productos, permitiendo respuestas precisas y actualizadas.

- Propósito: Sería un modelo afinado específicamente para EcoMarket, no un modelo de propósito general, para garantizar respuestas alineadas con los procedimientos y tono de la empresa.

- Interfase con canales de contacto: Se implementaría a través de APIs que permitan su uso en chat, email y redes sociales.

### 4. ¿Cuáles son los criterios que justifican esta elección?

- **Costo:** Aunque GPT-4 tiene costos asociados, su capacidad de integración y la reducción en el tiempo de respuesta justifican la inversión, ya que mejora la satisfacción del cliente y reduce cargas operativas.

- **Escalabilidad:** La infraestructura en la nube permite gestionar altos volúmenes de consultas sin necesidad de infraestructura local adicional.

- **Facilidad de integración:** La API de GPT-4 permite integrarse fácilmente con otros sistemas y canales de atención, facilitando una implementación rápida y efectiva.

- **Calidad de respuesta:** GPT-4 puede generar respuestas naturales, precisas y adaptadas al contexto, que son fundamentales para resolver consultas tanto simples como complejas.


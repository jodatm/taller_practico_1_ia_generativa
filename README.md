Taller práctico 1 

# Integrantes:
- Joshua Triana
- Andres Borrera

# Fase 1: Selección y Justificación del Modelo de IA para EcoMarket

### 1. ¿Qué tipo de modelo de IA generativa es el más adecuado?

Solución híbrida con un LLM GPT-4 afinado con datos de EcoMarket, conectado a las bases de datos y servicios internos para consultar información de pedidos y productos en tiempo real.

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

# Fase 2: Fortalezas, Limitaciones y Riesgos Éticos

### Fortalezas
- Reducción drástica del tiempo de respuesta en el ~80% de consultas repetitivas.
- Disponibilidad 24/7 con consistencia de tono y alineación con políticas.
- Mayor FCR al estar conectado a datos de pedidos y políticas vigentes.

### Limitaciones
- Dependencia de la calidad y actualización de datos internos.
- Ambigüedad en consultas sin contexto suficiente; requiere pedir aclaraciones.
- El ~20% de casos complejos (empatía/criterio) debe escalar a un agente humano.

### Riesgos éticos y mitigaciones
- Alucinaciones:
  - Mitigación: respuestas ancladas a la API/BD; plantillas con "No encontrado"; umbrales de confianza y escalamiento automático.
- Sesgo:
  - Mitigación: pruebas de equidad periódicas; revisión humana de muestras; ajuste de prompts/datos para reducir sesgos.
- Privacidad de datos:
  - Mitigación: minimización de PII en prompts; anonimización para entrenamiento; cifrado en tránsito/en reposo; control de accesos y retención limitada.
- Impacto laboral:
  - Mitigación: protocolo de escalamiento claro; capacitación de agentes para enfocarse en el 20% complejo; medición de carga para dimensionamiento del equipo.

# Fase 3: Ingeniería de Prompts

### 1) Prompt de estado de pedido
Objetivo: responder el estado de un pedido con datos del contexto.

Rol: actúa como agente de soporte de EcoMarket, claro y empático.

Contexto requerido: {order_context} con ≥10 pedidos (tracking_number, cliente, estado, ETA, link_tracking opcional).

Reglas/instrucciones:
- Usa únicamente la información del contexto.
- Si hay coincidencia por tracking_number: confirma el número, entrega estado y ETA; si "Enviado", incluye link de seguimiento.
- Si hay retraso: ofrece disculpa breve y causa si existe en el contexto.
- Si no hay coincidencia: indica "No encontrado" y solicita verificación de datos.
- No inventes datos ni completes campos ausentes.

Formato de salida recomendado:
- Resumen en 1 frase.
- Detalles: Estado, Fecha estimada, Seguimiento (URL o "No aplica").
- Cierre amable con oferta de ayuda.

Consulta ejemplo: "Hola, quiero saber dónde está mi pedido {{tracking_number}}."

### 2) Prompt de devolución de producto
Objetivo: guiar el proceso de devolución según políticas.

Rol: agente de soporte en devoluciones; tono claro y empático.

Políticas mínimas:
- Retornables: ropa, accesorios y hogar dentro de 30 días, sin usar y con etiquetas.
- No retornables: higiene personal y perecederos.

Reglas/instrucciones:
- Determina si el producto es retornable según categoría y plazo.
- Si es retornable: detalla pasos (plazo, estado del producto, etiqueta, canal de envío).
- Si no es retornable: explica el motivo con empatía y ofrece alternativa razonable (p. ej., descuento futuro).
- No inventes excepciones a las políticas.

Formato de salida recomendado:
- Decisión: Retornable / No retornable.
- Pasos (si aplica) o Motivo y Alternativa.

Solicitud ejemplo: "Hola, necesito devolver el artículo {{product_name}} del pedido #{{order_number}}."

### Evidencia y datos para pruebas
- Incluir un documento con ≥10 pedidos para el contexto (por ejemplo, data/pedidos.json).
- El repositorio debe contener la estructura mínima para ejecutar los prompts con ese contexto según lo indicado en el taller.


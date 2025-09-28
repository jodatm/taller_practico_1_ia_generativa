Taller práctico 1 

# Integrantes:
- Joshua Triana
- Andrés Borrero

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

- Modelo base: GPT-4, que se complementaría con un proceso de fine-tuning usando datos específicos de EcoMarket.

- Integración con bases de datos: El modelo se conectaría a una API que extrae información en tiempo real desde la base de datos de EcoMarket, como estados de pedido y detalles de productos, permitiendo respuestas precisas y actualizadas.

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

- Alucinaciones
  - Qué puede generar: respuestas inventadas sobre estados de pedido, políticas o características de productos; confirmaciones falsas; enlaces o números de seguimiento inexistentes.
  - Impacto de negocio: caída de CSAT/NPS, reclamos y reembolsos no planificados, pérdida de confianza de marca, riesgo legal por promesas incumplidas.
  - Mitigaciones:
    - Respuestas ancladas a fuentes: obligar llamadas a API/BD (RAG o funciones) y operar en modo "closed-book" cuando se requieran datos transaccionales.
    - Validaciones sintácticas y de negocio: esquemas de respuesta, campos obligatorios (estado, ETA, tracking). Si falta información: responder "No encontrado" y pedir verificación.
    - Umbrales de confianza y fallback: si la detección de intención/entidad tiene baja confianza, solicitar aclaración o escalar a humano.
    - Plantillas controladas para políticas y mensajes sensibles; versionado de políticas y controles de cambios.
    - Monitoreo: logging de fuentes consultadas, muestreo QA semanal, KPI de tasa de alucinación y acciones correctivas.

- Sesgo
  - Qué puede generar: trato desigual por idioma, región o perfil; variaciones injustificadas de tono/prioridad; recomendaciones desbalanceadas.
  - Impacto de negocio: daño reputacional, riesgo regulatorio, pérdida de clientes y percepción de discriminación.
  - Mitigaciones:
    - Curaduría y balance de datos de entrenamiento/contexto; guías de estilo inclusivas en prompts.
    - Pruebas de equidad pre y post despliegue (cohortes por idioma/segmento), con métricas de paridad y revisión de outliers.
    - Reglas de seguridad de contenido y filtros para lenguaje discriminatorio; ajuste de temperatura/top_p para reducir variabilidad indeseada.
    - Proceso de apelación: permitir que clientes y agentes reporten incidentes; registro y remediación con reentrenamiento/ajuste de prompts.

- Privacidad de datos
  - Qué puede generar: exposición de PII (direcciones, emails, historial), retención indebida de datos sensibles o uso no autorizado para entrenamiento.
  - Impacto de negocio: multas y sanciones (cumplimiento), incidentes de seguridad, pérdida de confianza y costos legales.
  - Mitigaciones:
    - Minimización de datos: enviar solo los campos necesarios; enmascaramiento/tokenización; scrubbing de PII en logs.
    - Seguridad: cifrado en tránsito (TLS) y en reposo; controles de acceso (RBAC), segregación de entornos y principios de menor privilegio.
    - Gobernanza: políticas de retención y borrado; acuerdos de procesamiento de datos (DPA); evitar que el proveedor use los datos para entrenamiento por defecto.
    - Monitoreo y auditoría: DLP, alertas de exfiltración, auditorías periódicas y pruebas de penetración del flujo de soporte.

- Impacto laboral
  - Qué puede generar: resistencia al cambio, ansiedad por reemplazo, deterioro de calidad si no hay escalamiento oportuno; pérdida de conocimiento tácito.
  - Impacto de negocio: rotación de personal, caída de calidad en el 20% complejo, afectación de CSAT y demoras en resolución de casos críticos.
  - Mitigaciones:
    - Human-in-the-loop: umbrales claros de escalamiento (intención/confianza), colas priorizadas y SLAs para casos complejos.
    - Capacitación y upskilling: entrenamiento en uso del sistema, nuevas funciones y protocolos; reconocimiento e incentivos.
    - Despliegue gradual y codiseño con agentes: pilotos controlados, feedback continuo y ajustes antes de escalar.
    - Medición continua: métricas de carga humana, calidad de resolución de casos complejos y encuestas de clima para decisiones de mejora.

# Fase 3: Ingeniería de Prompts

### 1) Prompt de estado de pedido (formato TOML)
```toml
[prompts]
instruction_prompt = """
Usa el contexto de pedidos en >>>ORDER_CONTEXT<<< y la consulta del usuario en >>>QUERY<<< para responder en español de forma clara y empática.

Sigue estos pasos:
1) Extrae el tracking_number desde >>>QUERY<<<.
2) Busca el pedido correspondiente en >>>ORDER_CONTEXT<<< por tracking_number.
3) Si se encuentra:
   - Confirma el tracking_number.
   - Entrega el estado actual y la fecha estimada (ETA).
   - Si el estado es "Enviado", incluye el enlace de seguimiento:
     https://tracking.ecomarket.co/track?id={{tracking_number}}
   - Si hay retraso indicado en el contexto, ofrece una disculpa breve y la causa si está disponible.
4) Si no se encuentra:
   - Indica "No encontrado" y solicita verificar el número o los datos del pedido.

Restricciones:
- Usa exclusivamente la información presente en >>>ORDER_CONTEXT<<<.
- No inventes datos; si un campo no está, indica "No disponible".

Formato de salida:
Resumen: <1 frase>
Estado: <estado>
Fecha estimada: <YYYY-MM-DD | No disponible>
Seguimiento: <URL | No aplica>
Cierre: <frase amable y breve>
"""

role_prompt = """
Eres un agente de soporte de EcoMarket. Responde de forma concisa, clara y empática.
"""
```

### 2) Prompt de devolución de producto (formato TOML)
```toml
[prompts]
instruction_prompt = """
Usa las políticas en >>>POLICY<<< y la consulta del cliente en >>>QUERY<<< para guiar el proceso de devolución en español.

Política mínima (ejemplo en >>>POLICY<<<):
- Retornables: ropa, accesorios y hogar dentro de 30 días, sin usar y con etiquetas.
- No retornables: higiene personal y perecederos.

Sigue estos pasos:
1) Identifica el producto y su categoría desde >>>QUERY<<<.
2) Determina si es retornable según >>>POLICY<<< y la fecha de compra si está disponible.
3) Si ES retornable: indica pasos claros (plazo, estado del producto, generación de etiqueta, canal de envío o punto de entrega).
4) Si NO es retornable: explica el motivo con empatía y ofrece una alternativa razonable (p. ej., descuento para próxima compra).

Restricciones:
- No inventes excepciones a la política.
- Si falta información esencial (categoría/fecha), solicita ese dato antes de continuar.

Formato de salida:
Decisión: <Retornable | No retornable>
Pasos: <lista numerada | No aplica>
Motivo: <si no es retornable>
Alternativa: <si aplica>
"""

role_prompt = """
Eres un agente de soporte especializado en devoluciones de EcoMarket. Mantén un tono claro, empático y resolutivo.
"""
```

### Evidencia y datos para pruebas
- Incluir un documento con ≥10 pedidos para el contexto (por ejemplo, data/pedidos.json) que alimente >>>ORDER_CONTEXT<<<.
- El repositorio debe contener la estructura mínima para ejecutar los prompts con ese contexto según lo indicado en el taller.


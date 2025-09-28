Taller práctico 1 

# Integrantes:
- Joshua Triana
- Andres Borrera

# Fase 1: Selección y Justificación del Modelo de IA para EcoMarket

### 1. ¿Qué tipo de modelo de IA generativa es el más adecuado?

Solución híbrida con un LLM (p. ej., GPT-4) afinado con datos de EcoMarket, conectado a las bases de datos y servicios internos para consultar información de pedidos y productos en tiempo real.

### 2. ¿Por qué este modelo y no otro?

- Alineado al caso: automatiza ~80% de consultas repetitivas y reduce el tiempo de respuesta de ~24h a segundos/minutos, manteniendo un tono empático.
- Comparación breve:
  - Modelos pequeños (BERT/DistilBERT): óptimos para clasificación/extracción, menos adecuados para respuestas conversacionales completas.
  - GPT-3.5: menor capacidad para seguir instrucciones complejas y mayor riesgo de errores en consultas con múltiples pasos frente a GPT-4.

### 3. ¿Cuál sería la arquitectura propuesta?

- LLM de generación.
- Capa de integración (API interna) que consulta BD de pedidos/productos y servicios de tracking en tiempo real.
- Orquestador y reglas: decide cuándo consultar datos, formatea respuestas y deriva el 20% de casos complejos a un agente humano.
- Canales: chat web, email y redes sociales mediante una API común.
- Seguridad y cumplimiento: no enviar PII innecesaria al modelo; registro y monitoreo de interacciones.

### 4. ¿Cuáles son los criterios que justifican esta elección?

- Costo/ROI: automatiza gran volumen de consultas repetitivas.
- Escalabilidad: manejo de miles de tickets/día sin infraestructura local.
- Facilidad de integración: APIs y servicios desacoplados.
- Calidad de respuesta: lenguaje natural, preciso y consistente con políticas de EcoMarket.

# Fase 2: Fortalezas, Limitaciones y Riesgos Éticos

### Fortalezas
- Reducción del tiempo de respuesta; disponibilidad 24/7; consistencia del tono y políticas.

### Limitaciones
- Casos complejos (20%) requieren criterio humano.
- Dependencia de la calidad y actualización de datos internos.

### Riesgos éticos y mitigaciones
- Alucinaciones: responder solo con datos recuperados de la API/BD; usar "no encontrado" cuando aplique.
- Privacidad de datos: anonimización de PII, cifrado en tránsito y en reposo, control de accesos y retención limitada.
- Sesgos: revisión periódica de outputs y ajustes; pruebas de equidad.
- Impacto laboral: protocolo claro de escalamiento y foco de agentes en casos complejos.

# Fase 3: Ingeniería de Prompts

### 1) Prompt de estado de pedido
Objetivo: consultar el estado usando un número de seguimiento y responder de forma clara y empática con datos del contexto.

Contexto requerido: documento con al menos 10 pedidos (número, cliente, estado, ETA, tracking). Ej.: data/pedidos.json.

Instrucciones sugeridas:
- Actúa como agente de soporte de EcoMarket.
- Usa únicamente la información del contexto de pedidos.
- Confirma el número de seguimiento.
- Devuelve estado actual y fecha estimada.
- Si "Enviado", incluye enlace de seguimiento: https://tracking.ecomarket.co/track?id={{tracking_number}}
- Si hay retraso, ofrece una breve disculpa y explica la causa si existe.
- Si no existe en el contexto, informa que no se encuentra y solicita verificación.

Consulta ejemplo:
Hola, quiero saber dónde está mi pedido {{tracking_number}}.

### 2) Prompt de devolución de producto

Política mínima:
- Retornables: ropa, accesorios y hogar dentro de 30 días, sin usar y con etiquetas.
- No retornables: higiene personal y perecederos.

Instrucciones sugeridas:
- Determina si el producto es retornable según la política.
- Si sí: indica pasos (plazo, estado del producto, etiqueta, canal de envío).
- Si no: explica con empatía por qué y ofrece alternativa razonable (por ejemplo, descuento futuro).

Solicitud ejemplo:
Hola, necesito devolver el artículo {{product_name}} del pedido #{{order_number}}.

### Evidencia y datos para pruebas
- Adjuntar el documento con ≥10 pedidos que el prompt usará como contexto.
- El repositorio debe incluir el material necesario para ejecutar los prompts según lo indicado en el taller.


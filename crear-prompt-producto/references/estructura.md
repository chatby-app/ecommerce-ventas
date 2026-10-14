# Estructura del archivo (formato Importar de Chatby Ecom)

El archivo es un JSON. El importador aplica **solo lo que venga**: lo que omitas, no se
toca. `chatby_product` y `name` son opcionales (al importar `name` se ignora porque el
producto ya existe). Escribe estos valores como texto; usa `\n` para los saltos de línea.

```json
{
  "chatby_product": 1,
  "name": "Nombre del producto (informativo, opcional)",
  "general": {
    "short_description": "..."
  },
  "entrenamiento": {
    "persona_role": "...",
    "skills": "...",
    "tech_sheet": "...",
    "restrictions": "...",
    "funciones": {
      "venta_finalizada": { "sale_desc": "...", "sale_prompt": "..." }
    }
  },
  "bienvenida": {
    "mensajes": ["bloque de texto 1", "bloque de texto 2"]
  }
}
```

## El "prompt" = todo lo que está dentro de `entrenamiento`
El sistema arma el prompt del bot concatenando, en orden, `persona_role` + `skills` +
`tech_sheet` + `restrictions` (y la función). Por eso cada campo tiene un **propósito**
distinto; no los mezcles.

| Campo | Propósito | Límite |
|---|---|---|
| `general.short_description` | **Descripción del producto** para responder comentarios o cuando se busca el producto. Autocontenida (precio, qué es, beneficios, modo de uso, web). | 1500 |
| `entrenamiento.persona_role` | **Quién es el agente**: nombre, rol, misión, personalidad, estilo y canal (WhatsApp/FB/IG). El "carácter". | 2000 |
| `entrenamiento.skills` | **Cómo vende**: el FLUJO de venta paso a paso + comportamientos (descubrimiento, pasos, objeciones, situaciones especiales, ejemplo de conversación). Es el campo más grande. | 15000 |
| `entrenamiento.tech_sheet` | **Ficha técnica / base de conocimiento del producto**: hechos que el agente consulta (descripción, ingredientes/materiales, beneficios, modo de uso, precauciones, tabla de precios, pago, envío/cobertura, FAQs). | 15000 |
| `entrenamiento.restrictions` | **Reglas duras / barreras**: lo que NUNCA debe hacer o afirmar (no inventar, no claims médicos, integridad de precios, no datos bancarios, etc.). | 2000 |
| `funciones.venta_finalizada.sale_desc` | Qué hace la función y CUÁNDO / cuándo NO usarla + parámetros. | 1000 |
| `funciones.venta_finalizada.sale_prompt` | Instrucciones que sigue la IA al ejecutarla (verificaciones, mensajes de confirmación, nunca mostrar el JSON). | 2000 |
| `bienvenida.mensajes[]` | **Mensaje(s) de apertura** del bot (parte del contexto). | Máx. 3 bloques de texto |

## Funciones
- La **única** función que va en `funciones` es **`venta_finalizada`** (se ejecuta cuando
  la venta queda cerrada y confirmada: registra el pedido).
- **`view_images` es interna y ya está integrada** (funciona similar a `venta_finalizada`
  pero para mostrar imágenes/videos/testimonios). **NO la agregues a `funciones`.** Solo
  **menciónala en el prompt** (en `skills`) para que el agente la invoque cuando el
  cliente pida fotos, videos o pruebas. Ej.: "Si el cliente pide fotos o videos, usa la
  función view_images."

## Variables de contexto del cliente (las inyecta el sistema)
El sistema agrega automáticamente, junto al prompt, un bloque con el contexto de la
conversación. **No lo escribas tú** en ningún campo; solo **usa estas variables** donde
sea natural dentro del prompt:

```
- Nombre: {{first_name}}
- Apellido: {{last_name}}
- Nombre Completo: {{user_name}}
- País: {{country}}
- Canal de entrada: {{channel}}
- WhatsApp: {{user_id}}  (solo referencia, no lo uses para deducir nada)
- Fecha y hora actual (NOW): {{now}}
```

Cómo aprovecharlas en el prompt:
- Saluda o personaliza con `{{first_name}}` / `{{user_name}}` en los momentos clave (no en
  cada mensaje).
- **Teléfono según el canal** (`{{channel}}`):
  - **WhatsApp:** el sistema ya tiene el teléfono (es el número de WhatsApp del cliente).
    **No lo pidas** ni lo listes en el formulario.
  - **Otros canales (Facebook, Instagram):** el sistema no tiene el número → pídelo. Usa
    `{{phone}}` si viene con valor; si está vacío, pídelo una vez y normaliza.
- **`{{now}}`** para fechas/entregas. Ojo: `{{now}}` viene en **UTC**; ajústala a la zona
  horaria del país del cliente (`{{country}}`) antes de decir horas o fechas.
- `{{country}}` / `{{channel}}` para adaptar modismo y mensajes.
- `{{user_id}}`: solo referencia; no deduzcas nada de él.

> Nota de ubicación: ese bloque de contexto se inyecta al final de la sección de
> entrenamiento (hoy tras `tech_sheet`; se recomienda tras `skills`, donde vive el flujo).
> Es trabajo del sistema, no de este archivo.

## Buenas prácticas de formato
- Usa `\n` para separar párrafos y `\n\n` entre bloques lógicos: el prompt se lee mejor.
- Usa *negritas* de WhatsApp (`*texto*`) para precios y datos clave dentro de los mensajes
  que el agente enviará.
- Mantén cada campo dentro de su límite. Si `skills` o `tech_sheet` se acercan al tope,
  prioriza lo que más impacta la venta y mueve detalle fino a FAQs.

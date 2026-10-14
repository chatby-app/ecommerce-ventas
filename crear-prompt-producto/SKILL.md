---
name: crear-prompt-producto
description: >-
  Convierte la información de un producto —pegada como texto, en una imagen, en un link o
  en un .json exportado— en el prompt de venta COMPLETO para el bot vendedor de Chatby Ecom,
  y lo entrega como archivo JSON listo para Importar en el editor del producto. Arma toda la
  estructura: la descripción del producto (short_description), el entrenamiento del agente
  (persona/rol, skills y flujo de ventas, ficha técnica, restricciones) con la función de
  cierre venta_finalizada, y la bienvenida. Úsalo SIEMPRE que el usuario quiera crear, armar,
  mejorar o generar el prompt, el entrenamiento, la persona, el flujo de ventas, la ficha, las
  restricciones o la bienvenida de un producto o agente para vender y cerrar pedidos por
  WhatsApp, Facebook o Instagram — aunque no diga "skill" ni "JSON" —, o convertir la info de
  su producto en un bot que venda. NO lo uses para generar imágenes desde un prompt, para copy
  de anuncios o SEO, para chatbots de soporte, ni para la API de pedidos de Chatby Ecom.
---

# Crear prompt de producto (Chatby Ecom)

Tu trabajo es convertir la información de un producto en un **prompt de ventas
completo** que haga que el bot de Chatby Ecom venda de forma natural y efectiva por
WhatsApp, Facebook e Instagram — y entregarlo como un **archivo JSON** que el usuario
pega con el botón **Importar** del editor del producto.

El objetivo real: una conversación que se sienta **humana** para el cliente final y, a
la vez, una **máquina de ventas** (descubre, resuelve objeciones, pide datos sin
fricción, cierra y registra el pedido).

## Flujo de trabajo

### 1. Reúne la información del producto
Antes de escribir nada, necesitas materia prima. El usuario puede dártela de varias formas
—acéptalas todas:

- **Texto pegado** en el chat.
- **Imágenes** (foto del producto, etiqueta, flyer, captura de la página o del catálogo):
  **léelas** y extrae lo que contengan (nombre, precio, ingredientes, beneficios, etc.).
- **Un link** a la página del producto: **ábrelo** para extraer la info (usa tu capacidad de
  navegación/fetch si la tienes). **Si no puedes abrirlo** (sin acceso web, página dinámica o
  bloqueada), **pídele al usuario** que pegue el contenido o comparta una captura — no inventes
  lo que no pudiste leer.
- **Un `.json` exportado** de otro producto, como base/plantilla.

Lo esencial a reunir:

- **Marca / empresa** y **nombre del asesor** (p. ej. "Daniela").
- **País / mercado** (define el tono y el modismo) y **moneda**.
- **Producto**: qué es, para qué sirve, presentación, **variantes** (talla/color/…), web.
- **Precios**: por unidad y **tabla por cantidad** (combos/descuentos por volumen si hay).
- **Pago**: métodos (contraentrega, anticipado, etc.).
- **Envío y cobertura**: tiempos, costo, zonas sin cobertura, retiro en oficina.
- **Ficha**: ingredientes/materiales, modo de uso, beneficios, **precauciones/legal**.
- **FAQs** y **restricciones** (lo que NO puede afirmar: p. ej. claims médicos).
- **Obsequios/combos** si aplican.

Datos sensibles (sobre todo **precios** y **cobertura**) que leíste de una imagen o una
página: **confírmalos con el usuario** antes de generar, por si se leyeron mal. Si faltan
datos **clave para vender** (precio, cobertura, qué es), pídelos. No interrogues de más:
pregunta solo los vacíos importantes y sigue.

### 2. Pregunta el modo (orientación)
Cuando ya tengas suficiente información del producto, **pregunta al usuario**:

> "¿Quieres que arme yo todo el prompt (persona, flujo de ventas, ficha, restricciones,
> la función *venta_finalizada* y la bienvenida), o prefieres indicarme cómo debe
> comportarse y vender?"

- **Si el usuario quiere que lo armes tú** → construye un prompt completo y de alta
  conversión guiándote por `references/manual-de-venta.md` (la cartilla de venta
  destilada de prompts reales que funcionan), adaptado al producto y al mercado.
- **Si el usuario te indica cómo debe funcionar** → respeta su definición, pero igual
  estructúrala en los campos correctos y aplícale la base de calidad del manual
  (tono natural, flujo ordenado, cierre, formato del resumen).

### 3. Arma el JSON
Lee `references/estructura.md` para el esquema exacto, el rol de cada campo, sus
**límites de caracteres** y detalles importantes (función `view_images` interna,
variables de contexto del cliente que inyecta el sistema, dónde va cada cosa).

Reglas que no se te pueden olvidar:
- En `funciones` va **solo `venta_finalizada`**. `view_images` es **interna** (ya está
  integrada): NO la agregues a `funciones`; solo menciónala en el prompt para cuando el
  cliente pida fotos/videos/testimonios.
- Las **variables del cliente** (`{{first_name}}`, `{{user_name}}`, `{{country}}`,
  `{{channel}}`, `{{user_id}}`, `{{now}}`, y `{{phone}}` en canales que no son WhatsApp) las
  **inyecta el sistema**: no las re-declares; solo refiérelas donde sea natural. Clave:
  **el teléfono depende del canal** — en WhatsApp el sistema ya lo tiene (no lo pidas), en
  otros canales usa `{{phone}}` o pídelo; y `{{now}}` viene en UTC (ajústala al país). Ver
  `references/estructura.md` para el detalle.
- `short_description` es la **descripción del producto** para responder comentarios o
  cuando se busca el producto: debe ser completa y entenderse por sí sola.
- `bienvenida.mensajes` es el **mensaje de apertura** del bot (1 a 3 bloques de texto);
  es parte del contexto.
- Respeta los **límites de caracteres** de cada campo (ver estructura).

`references/ejemplo-producto.json` es un ejemplo ficticio y completo en el formato exacto:
úsalo como referencia de "cómo se ve un buen resultado" (no lo copies literal; adáptalo).

### 4. Entrega el archivo
Escribe el resultado en un archivo **`.json`** (nómbralo con el producto, p. ej.
`producto-<nombre>.json`) y entrégaselo al usuario. Dile que lo cargue con el botón
**Importar (✨)** en el editor del producto: General/Entrenamiento/Funciones quedan
listos para que presione **Guardar**, y la Bienvenida se guarda al importar.

Antes de entregar, repasa: ¿suena humano y natural? ¿tiene un flujo de venta claro que
lleva al cierre? ¿respeta precios/restricciones sin inventar? ¿cada campo está dentro de
su límite? Si algo quedó flojo, mejóralo.

## Mapa de archivos de referencia
- `references/estructura.md` — esquema JSON, rol y límite de cada campo, `view_images`,
  variables de contexto, dónde va cada cosa. **Léelo siempre antes de armar el JSON.**
- `references/manual-de-venta.md` — la cartilla para escribir un prompt que vende natural
  (persona, tono por mercado, humanización, flujo paso a paso, objeciones, cierre,
  formato del resumen, `venta_finalizada`). **Léelo cuando tú armes el prompt.**
- `references/ejemplo-producto.json` — ejemplo ficticio completo en el formato exacto.

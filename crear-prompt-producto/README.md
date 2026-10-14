# crear-prompt-producto

SKILL de Claude que convierte la información de un producto en el **prompt de venta
completo** para un bot de ventas por WhatsApp, Facebook e Instagram, y lo entrega como un
**archivo JSON** listo para importar.

Genera toda la estructura: descripción del producto, persona del agente, flujo de ventas,
ficha técnica, restricciones, la función de cierre `venta_finalizada` y el mensaje de
bienvenida. El objetivo: una conversación natural para el cliente final y una verdadera
máquina de ventas.

## Instalar

**Claude Code** — copia esta carpeta dentro de `.claude/skills/` de tu proyecto (o de
`~/.claude/skills/` para tenerla en todos):

```bash
git clone <URL-del-repo>
cp -r crear-prompt-producto ~/.claude/skills/
```

**Claude.ai / app** — descarga el archivo empaquetado `crear-prompt-producto.skill` y, en la
tarjeta que aparece, pulsa **Save skill** para instalarla en tu perfil.

## Usar

1. Abre Claude y dile algo como: *"Ayúdame a crear el prompt de venta de mi producto"* (o
   invoca la skill por su nombre). Pégale la información del producto, o adjúntale un `.json`
   exportado de otro producto como base.
2. La skill te preguntará si quieres que **ella arme todo el prompt** o si **tú le indicas
   cómo debe comportarse**.
3. Te entrega un archivo `.json`.
4. En el editor del producto (Chatby Ecom) pulsa **Importar (✨)**: General, Entrenamiento y
   Funciones quedan listos para **Guardar**; la Bienvenida se guarda al importar.

## Qué hay dentro

- `SKILL.md` — el flujo que sigue Claude.
- `references/estructura.md` — esquema JSON, rol y límite de cada campo, variables de contexto.
- `references/manual-de-venta.md` — cartilla para escribir un prompt que vende natural.
- `references/ejemplo-producto.json` — ejemplo ficticio completo en el formato exacto.

El archivo JSON tiene esta forma (lo que no venga, no se toca al importar):

```json
{
  "general": { "short_description": "..." },
  "entrenamiento": {
    "persona_role": "...", "skills": "...", "tech_sheet": "...", "restrictions": "...",
    "funciones": { "venta_finalizada": { "sale_desc": "...", "sale_prompt": "..." } }
  },
  "bienvenida": { "mensajes": ["..."] }
}
```

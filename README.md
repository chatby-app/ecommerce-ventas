# ecommerce-ventas

SKILL de Claude para convertir la información de un producto en el **prompt de venta
completo** de un bot vendedor por WhatsApp, Facebook e Instagram, listo para importar.

## Skill incluida

- **[`crear-prompt-producto/`](crear-prompt-producto/)** — a partir de la info del producto
  (texto, imagen, link o un `.json` exportado) arma toda la estructura: descripción del
  producto, persona del agente, flujo de ventas, ficha técnica, restricciones, la función de
  cierre `venta_finalizada` y la bienvenida. Entrega un `.json` listo para **Importar** en el
  editor del producto.

## Instalar

**Claude Code** — copia la carpeta de la skill en tu `.claude/skills/`:

```bash
git clone https://github.com/chatby-app/ecommerce-ventas.git
cp -r ecommerce-ventas/crear-prompt-producto ~/.claude/skills/
```

**Claude.ai / app** — descarga [`crear-prompt-producto.skill`](crear-prompt-producto.skill)
(botón **Download**) y, en la tarjeta que aparece al abrirlo en Claude, pulsa **Save skill**.

Más detalle en el [README de la skill](crear-prompt-producto/README.md).

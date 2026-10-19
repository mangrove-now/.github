# mangrove-now/.github

Configuración compartida de la org **mangrove-now**.

- **`.github/ISSUE_TEMPLATE/`**: templates de issue (Bug, Feature, Task). GitHub los usa en todo repo de la org que no tenga los suyos, y cada ticket nuevo entra solo al [board](https://github.com/orgs/mangrove-now/projects/1).
- **[`BOARD.md`](BOARD.md)**: cómo funciona el board y cómo escribir un ticket.
- **`plugins/mangrove-board/`**: plugin de Claude Code para crear y mover tickets siguiendo esas convenciones.

## Instalar el plugin de Claude Code

```
/plugin marketplace add mangrove-now/.github
/plugin install mangrove-board@mangrove
```

Requiere `gh` con los scopes `project` y `repo` (`gh auth refresh -s project`).

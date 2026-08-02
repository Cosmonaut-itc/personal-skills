# CLAUDE.md global

## Delegación y elección de modelo

Eres el **orquestador**: delegas la ejecución y vetas el resultado final.

| modelo        | costo | inteligencia | gusto |
|---------------|-------|--------------|-------|
| gpt-5.6-sol   | 8     | 9            | 6     |
| gpt-5.6-terra | 9     | 8            | 5     |
| gpt-5.6-luna  | 10    | 7            | 4     |
| sonnet-5      | 5     | 5            | 7     |
| opus-5        | 4     | 8            | 8     |

Elige siempre dentro de esta tabla. El **presupuesto** real son los tokens
Claude (las variantes gpt-5.6 son gratis en la práctica): usa gpt-5.6-luna para
tareas rápidas, mecánicas y bien delimitadas; gpt-5.6-terra como opción por
defecto para el trabajo cotidiano; y gpt-5.6-sol para problemas difíciles y
reviews. Gasta Claude donde el gusto o el juicio difícil pagan. En conflicto
sobre algo que se embarca: inteligencia > gusto > costo. Tienes permiso
permanente de escalar a un modelo mejor cuando el output no da la talla, sin
preguntar.

UI/UX: primero un prototipo de fidelidad baja/media aprobado por el dueño
(diseña opus-5 si no existe prototipo ni guía); implementa gpt-5.6-terra.

Reviews: primero gpt-5.6, siempre en la variante `gpt-5.6-sol` con esfuerzo
`high`; después tú lees el diff completo y vetas tanto el cambio como sus
hallazgos. Un cambio de **alto riesgo** lleva además un pase opus-5
independiente (auth/permisos, migraciones de datos, releases, y lo que el
CLAUDE.md del proyecto marque como alto riesgo).

Mecánica para despachar las variantes gpt-5.6 (flags de Codex CLI, wrapper para
workflows, comandos del plugin): lee `~/.claude/docs/codex-delegation.md` antes
de tu primer despacho de la sesión.

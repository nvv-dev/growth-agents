# growth-agents

Un equipo de marketing pequeño como subagentes de Claude Code: copy, contenido, growth, revisión legal y precios. Cada agente tiene una responsabilidad, las herramientas que necesita y límites escritos de lo que **no** puede hacer. Hay una persona dueña que aprueba todo lo que se publica, se envía, cuesta dinero o cambia precios.

*English version below.*

## Objetivo

Que el marketing hecho con agentes parta de hechos aprobados y llegue a la persona dueña listo para decidir: borradores con fuentes y puntajes, experimentos con métrica y regla de decisión, y preguntas precisas para un abogado. Ningún agente publica, envía, gasta dinero ni cambia precios.

## Agentes

| Agente | Responsabilidad | Modelo | Lee antes de escribir | Nunca |
|---|---|---|---|---|
| [copywriter](agents/copywriter.md) | Copy de páginas, microcopy y correos a partir del contexto, la voz y el mapa de palabras clave; llena el paso de "copy" del flujo de un sitio | Sonnet | `product-context.md`, `voice.md` | Inventar afirmaciones, cifras o testimonios; publicar o enviar |
| [content-strategist](agents/content-strategist.md) | Matriz de contenidos, borradores de posts y carruseles, y `post-review` antes de entregar | Sonnet | `product-context.md`, `voice.md` | Publicar; usar una noticia sin fuente primaria |
| [growth-marketer](agents/growth-marketer.md) | Fichas de tienda, planes de lanzamiento, experimentos de conversión y lectura de métricas | Sonnet | `product-context.md` | Cambiar precios, gastar dinero o publicar |
| [legal-reviewer](agents/legal-reviewer.md) | Revisa términos, políticas y contratos con `legal-pages-check` y la base `legal/` | Sonnet | `legal/` | Dar asesoría definitiva, aprobar firmas o publicar |
| [pricing-analyst](agents/pricing-analyst.md) | Por producto o servicio: piso de costo, rango de la competencia con fuente y fecha, techo de valor y una recomendación con su supuesto y cómo probarla (`pricing-research`) | Sonnet | `product-context.md` | Publicar o cambiar precios, dar asesoría tributaria, extraer datos contra los términos de uso de un sitio |

`copywriter`, `content-strategist`, `legal-reviewer` y `pricing-analyst` no tienen `Bash` a propósito: escriben borradores en archivos y no pueden llamar a APIs de publicación. `growth-marketer` usa `Bash` solo para leer exportaciones de métricas. Los límites se refuerzan con los hooks de [agent-guardrails](https://github.com/nvv-dev/agent-guardrails).

## Flujo

```text
product-context (y voice-profile) → aprobación
sitio:      seo-geo-specialist (mapa de palabras clave) → ui-ux-designer → copywriter → aprobación
            → frontend-engineer → auditoría SEO/GEO → la persona publica
contenido:  content-strategist (matriz → borradores → post-review) → la persona aprueba y publica
lanzamiento: growth-marketer (ficha + launch-checklist) + legal-reviewer (páginas legales) → la persona decide
precios:    pricing-analyst (pricing-research: piso, rango, techo, recomendación y prueba) → la persona decide
```

Los agentes usan las skills del plugin [growth-workflow](https://github.com/nvv-dev/growth-workflow). Los de diseño, SEO/GEO y coordinación están en [dev-agents](https://github.com/nvv-dev/dev-agents); su `coordinator` enruta las tareas de marketing a estos agentes cuando están instalados.

## Instalación

```text
/plugin marketplace add nvv-dev/claude-plugins
/plugin install growth-agents@nvvdev
/plugin install growth-workflow@nvvdev
/plugin install agent-guardrails@nvvdev
```

O copia los archivos de `agents/` que quieras a `.claude/agents/` en tu repo y ajústalos.

## Cómo adaptarlos

- **Datos del negocio:** no edites los perfiles con ellos. Corre `product-context` y `voice-profile` para crear `.agents/product-context.md` y `.agents/voice.md`.
- **Modelo:** Sonnet en todos. Cámbialo en el campo `model` según tu plan.
- **Herramientas:** no agregues `Bash` ni herramientas de publicación a copywriter, content-strategist o legal-reviewer.
- **País:** `legal-pages-check` trae Colombia (Ley 1581 de 2012) como ejemplo; agrega la lista de tu país con sus fuentes oficiales.

## Licencia

MIT. Ver [CREDITS.md](CREDITS.md).

---

## English

A small marketing team as Claude Code subagents: copywriter, content strategist, growth marketer, legal reviewer and pricing analyst. Each has one responsibility, the tools it needs, and written limits. None publishes, sends, spends money or changes prices; one human owner approves all of that. The copywriter and content strategist read `.agents/product-context.md` and `.agents/voice.md` before writing; the copywriter, content strategist, legal reviewer and pricing analyst have no `Bash` on purpose.

Install with `/plugin marketplace add nvv-dev/claude-plugins` and `/plugin install growth-agents@nvvdev` (together with `growth-workflow` and `agent-guardrails`), or copy the files in `agents/` into your repo's `.claude/agents/`. The legal reviewer is not a lawyer and says so in every report. License: MIT.

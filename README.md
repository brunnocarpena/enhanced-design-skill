# Enhanced Design

O **maestro de web design** para Claude Code. Um comando só (`/enhanced-design`) que roda o pipeline inteiro de design na ordem que um estúdio de verdade seguiria — em vez de você lembrar de rodar cinco skills separadas na sequência certa.

```
raciocinar → montar design system → aplicar os padrões da casa →
construir → auditar (performance · acessibilidade · anti-slop · congruência) →
decidir deploy
```

Inspirada na thread do [@nett0eth](https://x.com/nett0eth/status/2076763781057663040) ("As 5 melhores Skills de Web Design pra Claude"). Em vez de escolher **uma** skill de design, encadeia todas — porque elas se somam.

## O que ela orquestra

| Fase | Skill | Papel |
|------|-------|-------|
| 1 | `ui-ux-pro-max` + `frontend-design` | raciocínio + direção estética distinta (anti-template) |
| 2 | `seek-patterns` | acabamentos consolidados dos projetos anteriores |
| 3 | (construção) | implementar seguindo o design system |
| 4a | `performance-audit` | PageSpeed / Core Web Vitals sem quebrar conversão |
| 4b | Web Design Guidelines (fallback embutido) | acessibilidade / interface (foco, ARIA, zoom, paste) |
| 4c | Impeccable (fallback embutido) | anti-slop (46 regras contra o cheiro de UI de IA) |
| 4d | `congruence` | copy/metadata batem com o que o código faz |

Skills não instaladas (Impeccable, Web Design Guidelines, Astryx) têm **fallback inline** em [`references/`](references/) — o maestro nunca finge que rodou algo que não existe.

## Modos

- `--mode=build` — interface nova do zero (pipeline completo).
- `--mode=refine` — melhorar interface existente (pula fundação pesada).
- `--mode=audit` — só QA antes de subir (fase 4 + gate de deploy).

## Uso

```
/enhanced-design                         # infere modo pelo contexto da sessão
/enhanced-design --mode=build            # página nova
/enhanced-design https://exemplo.com --mode=audit --target=mobile
```

## Estrutura

```
enhanced-design/
├── SKILL.md
├── README.md
└── references/
    ├── skill-map.md               # o que cada skill faz, links, como instalar as que faltam
    ├── reasoning-fallback.md      # plano B da fase 1 (ui-ux-pro-max / frontend-design)
    ├── house-rules.md             # plano B da fase 2 (seek-patterns) — padrões da casa
    ├── slop-rules.md              # plano B da fase 4c (Impeccable)
    └── web-interface-guidelines.md# plano B da fase 4b (Web Design Guidelines / Vercel Labs)
```

## Base estrutural

Modelada nas skills [`congruence`](https://github.com/brunnocarpena/congruence-skill) e [`performance-audit`](https://github.com/brunnocarpena/performance-audit-skill): mesmo formato de frontmatter com TRIGGER/SKIP, princípio central, modos de operação, workflow numerado, tabelas Red Flags e Excuse|Reality, gate pré-deploy e regras invioláveis.

## Instalar as skills que faltam (opcional, elevam o resultado)

- **Impeccable:** `npx impeccable install` (Node ≥ 22.12) — impeccable.style
- **Web Design Guidelines:** parte de `vercel-labs/agent-skills`
- **Astryx:** design system da Meta (React + StyleX)

Criado com `skill-creator` / `skill-generator`.

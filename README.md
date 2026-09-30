# Enhanced Design

O **maestro de web design** para Claude Code. Um comando só (`/enhanced-design`) que roda o pipeline inteiro na ordem que um estúdio seguiria, em vez de você lembrar de rodar várias skills na sequência certa.

```
ler o brief (Design Read) → decidir direção (dials + 2 passes) →
montar o sistema (motor ui-ux-pro-max + design.md) → padrões da casa →
construir → auditar (performance · interface · anti-slop · congruência) →
decidir deploy
```

Inspirada na thread do [@nett0eth](https://x.com/nett0eth/status/2076763781057663040) ("As 5 melhores Skills de Web Design pra Claude"). Desde 29/09/2026 também incorpora o conhecimento destilado da [taste-skill](https://github.com/Leonxlnx/taste-skill) (MIT, commit 87da8a4), do motor Python do `ui-ux-pro-max` e da versão plugin do `frontend-design`, sempre filtrado pelas regras da casa.

## O que ela orquestra

| Fase | Peça | Papel |
|------|------|-------|
| 0 | Design Read | ler tipo de página, público e ação principal antes de desenhar |
| 1 | motor `ui-ux-pro-max` + `frontend-design` + presets | dials, sistema de tokens, direção anti-template, design.md |
| 2 | `seek-patterns` | acabamentos consolidados da casa |
| 3 | construção | implementar pelo design.md, motion pelo dial (micro-interação, GSAP ou modo cinematográfico opcional) |
| 4a | `performance-audit` | Core Web Vitals sem quebrar conversão |
| 4b | `web-design-guidelines` + checklist | interface, toque, formulários, estados, a11y |
| 4c | `impeccable` + checagens mecânicas | anti-slop contado, não achado |
| 4d | `congruence` | copy/metadata batem com o código |

## Modos

- `--mode=build` - interface nova (pipeline completo).
- `--mode=refine` - melhorar o que existe, pelo protocolo de redesign (sem reescrever, sem mexer em URL/form/tracking).
- `--mode=audit` - só QA + gate de deploy.

## Uso

```
/enhanced-design                         # infere o modo pelo contexto
/enhanced-design --mode=build            # página nova
/enhanced-design https://exemplo.com --mode=audit --target=mobile
```

## Estrutura

```
enhanced-design/
├── SKILL.md
├── README.md
└── references/
    ├── aesthetic-direction.md      # Design Read, dials, 2 passes, defaults a evitar, escrita de interface
    ├── ui-ux-pro-max-engine.md     # protocolo do motor Python (search.py) + filtro da casa
    ├── style-presets.md            # 7 presets para projeto sem identidade
    ├── design-md-template.md       # formato do design.md (Stitch adaptado)
    ├── redesign-protocol.md        # modo refine
    ├── fix-requests.md             # corrigir sem quebrar: sintomas, auditoria de print, Preserve/Não use/Validação
    ├── image-to-code.md            # print/Claude Design/Figma → código; geração de referência
    ├── brandkit.md                 # marca e logo do zero
    ├── motion-patterns.md          # defaults de motion, proibições, ponteiros
    ├── motion-craft.md             # Emil Kowalski: tokens, receitas, checklist de animação
    ├── gsap-scroll.md              # GSAP + ScrollTrigger + plugins (oficial)
    ├── cinematic-mode.md           # modo cinematográfico: 6 motores 3D/scroll e guarda-corpos
    ├── house-rules.md              # regras da casa (vencem tudo)
    ├── slop-rules.md               # anti-slop + checagens mecânicas
    ├── web-interface-guidelines.md # interface e acessibilidade
    ├── reasoning-fallback.md       # plano B da fase 1
    └── skill-map.md                # origem de cada peça, o que foi absorvido e descartado
```

## Hierarquia

Regras da casa > identidade existente (Claude Design, marca, design.md) > brief > conhecimento absorvido.

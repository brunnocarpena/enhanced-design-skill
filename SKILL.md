---
name: enhanced-design
description: Use sempre que for CRIAR, REDESENHAR ou fazer QA de qualquer interface web/mobile (landing page, página de captura ou de vendas, dashboard, app, componente, formulário) e o resultado não pode sair genérico. TRIGGER quando o usuário diz "enhanced design", "design caprichado", "roda tudo de design", "página premium", "não quero genérico", "cara de IA", "taste", "redesign", "refazer o visual", "polir a UI", "deixa lindo", "web design", "landing bonita", "brandkit", "logo", "transformar print/imagem em código", "animação", "micro-interação", "scroll", "GSAP", "site cinematográfico", "3D", "WebGL", "Three.js", "Spline", "tipo Apple"; quando a sessão cria ou edita hero, seções, componentes, formulários, temas ou tipografia; ANTES de qualquer deploy de página pública. SKIP em mudança só de backend/API/schema sem impacto visual, ou quando o usuário pede explicitamente UMA skill específica ("roda só o performance-audit").
argument-hint: [escopo-ou-url?] [--mode=build|refine|audit] [--target=mobile|desktop|both]
allowed-tools: Read, Grep, Glob, Skill, Task, WebFetch, Bash(python3:*), Bash(cd:*), Bash(git diff:*), Bash(git status:*), Bash(ls:*), Bash(find:*), Bash(wc:*)
---

# Enhanced Design

O **maestro de web design**. Encadeia o pipeline que um estúdio seguiria: ler o brief, decidir a direção, montar o sistema, aplicar o acabamento da casa, construir e só subir depois de auditar. Orquestra as skills instaladas (ui-ux-pro-max, frontend-design, seek-patterns, performance-audit, impeccable, web-design-guidelines, congruence) e carrega, em `references/`, o conhecimento destilado da taste-skill, do motor do ui-ux-pro-max, do frontend-design, do Emil Kowalski (motion), das skills oficiais do GSAP e de fontes de 3D/scrollytelling (modo cinematográfico).

## Princípio central

```
DESIGN BOM NÃO É SORTE DO MODELO A CADA COMPONENTE.
É UMA DIREÇÃO DECIDIDA ANTES DO CÓDIGO, UM SISTEMA CONSISTENTE
E UM QA QUE NÃO DEIXA SUBIR NADA GENÉRICO, LENTO OU INCONGRUENTE.
```

**Hierarquia de autoridade** (a de cima vence sempre):
1. Regras da casa ([house-rules.md](references/house-rules.md) + RULES-GLOBAL).
2. Identidade existente: Claude Design/Figma (porte fiel), marca, design.md do projeto.
3. O brief do usuário.
4. Conhecimento absorvido (presets, dials, motor, checagens).

## Mapa das referências

| Quando | Ler |
|---|---|
| Fase 1, sempre em build | [aesthetic-direction.md](references/aesthetic-direction.md) (Design Read, dials, 2 passes, defaults a evitar) |
| Fase 1, gerar sistema | [ui-ux-pro-max-engine.md](references/ui-ux-pro-max-engine.md) (motor Python via Bash) |
| Projeto sem identidade | [style-presets.md](references/style-presets.md) (P1-P7) |
| Brief pede marca/logo | [brandkit.md](references/brandkit.md) |
| Saída da Fase 1 | [design-md-template.md](references/design-md-template.md) |
| Modo refine | [redesign-protocol.md](references/redesign-protocol.md) |
| Tem print, Claude Design, Figma ou vai gerar referência | [image-to-code.md](references/image-to-code.md) |
| Qualquer motion (defaults e proibições) | [motion-patterns.md](references/motion-patterns.md) |
| Micro-interação ou revisar animação | [motion-craft.md](references/motion-craft.md) |
| Scroll narrativo, pin, scrub, SplitText, SVG | [gsap-scroll.md](references/gsap-scroll.md) |
| MOTION 9-10, 3D, WebGL, frames, scroll-world | [cinematic-mode.md](references/cinematic-mode.md) |
| Fase 2 | [house-rules.md](references/house-rules.md) |
| Fase 4b / 4c | [web-interface-guidelines.md](references/web-interface-guidelines.md) / [slop-rules.md](references/slop-rules.md) |
| Motor falhou ou skill faltando | [reasoning-fallback.md](references/reasoning-fallback.md) |
| Origem de cada peça | [skill-map.md](references/skill-map.md) |

Ler só a referência da fase atual, não todas de uma vez.

## Modos

- **build**: interface nova. Fases 0 → 5 completas.
- **refine**: melhorar o que existe. Fase 0 → [redesign-protocol.md](references/redesign-protocol.md) → Fase 2 → 4 → 5. Nunca reescrever nem migrar stack.
- **audit**: só Fase 4 + gate. Não redesenha.

Se ambíguo, perguntar **uma vez**.

## Workflow

### Fase 0 - Enquadrar
1. Ler o escopo (URL, arquivos, `git status`/`git diff`) e contar os entregáveis pedidos (quantas páginas, seções, telas). Entregar todos; nada de "exemplo de uma e o resto igual".
2. Detectar o ponto de partida: existe Claude Design/Figma (→ porte fiel, [image-to-code.md](references/image-to-code.md) seção 1-3), site existente (→ refine), ou greenfield (→ build).
3. Escrever o **Design Read** em uma linha (formato em [aesthetic-direction.md](references/aesthetic-direction.md) seção 1): "Lendo isto como: ... Trabalho principal da página: ...". Ambíguo → uma pergunta.
4. Target: mobile-first sempre; desktop é complemento.

### Fase 1 - Direção e sistema (build; refine só em overhaul)
5. Fixar os **dials** VARIANCE / MOTION / DENSITY pelo preset do tipo de página e justificar ([aesthetic-direction.md](references/aesthetic-direction.md) seção 2).
6. Rodar o **motor do ui-ux-pro-max** pelo protocolo de [ui-ux-pro-max-engine.md](references/ui-ux-pro-max-engine.md) (`--domain product` → `--design-system` → aprofundar → `--stack` → `--domain ux`). Filtrar a saída pela tabela da casa. Se o usuário quiser a skill inteira, `Skill: ui-ux-pro-max`.
7. Projeto sem identidade → escolher um preset de [style-presets.md](references/style-presets.md) ou declarar "sem preset", e justificar. Brief pede marca → [brandkit.md](references/brandkit.md) antes de qualquer tela.
8. **Passe 1**: plano de tokens (4-6 hex, 1-2 famílias com papel, wireframe ASCII, 2-3 princípios). **Passe 2**: criticar o plano contra o brief e contra os defaults de calibração; dizer o que mudou. Só então codar. Para direção estética mais funda, `Skill: frontend-design:frontend-design`.
9. Registrar no `design.md` da raiz do projeto (greenfield: na pasta onde o projeto vai nascer; teste sem projeto: scratchpad) no formato de [design-md-template.md](references/design-md-template.md) (atualizar se já existe). Um DS por projeto: se já existe um (shadcn, Astryx, o da marca), ele é a fundação.

### Fase 2 - Padrões da casa
10. `Skill: seek-patterns` (fallback: [house-rules.md](references/house-rules.md)). CTA CAIXA ALTA + shimmer, grade auto-fit, sem max-width de página, padding de seção único, primeira dobra visível, fotos distintas, ícone oficial do WhatsApp, UX de formulário, ícone em cima do texto no mobile.

### Fase 3 - Construção
11. Implementar seguindo o design.md. Motion pelo dial ([motion-patterns.md](references/motion-patterns.md); micro-interação pelas receitas e checklist de [motion-craft.md](references/motion-craft.md); scroll narrativo por [gsap-scroll.md](references/gsap-scroll.md); modo cinematográfico só com aceite registrado, por [cinematic-mode.md](references/cinematic-mode.md)). **Zero número fake**, **sem travessão** em texto user-facing, **metadata própria** (title = H1, description = sub + data, og:image da própria página).
12. Imagem que falta vira slot marcado com proporção, listado no resumo. Geração paga só com ok do usuário.
13. Dependência nova → `supply-chain-guard` antes (versão exata, nunca `@latest`).

### Fase 4 - QA (sempre, em qualquer modo)
- **4a Performance**: `Skill: performance-audit`. LCP/CLS/INP no verde sem quebrar tracking/UTM.
- **4b Interface/a11y**: `Skill: web-design-guidelines` + checklist de [web-interface-guidelines.md](references/web-interface-guidelines.md) (toque, formulários, estados, navegação, gráficos).
- **4c Anti-slop**: `Skill: impeccable` (audit/quieter) + as **checagens mecânicas** de [slop-rules.md](references/slop-rules.md), reportando as contagens (eyebrows, zigzag, famílias de layout, elementos do hero, travessões).
- **4d Congruência**: `Skill: congruence`.
- Autocrítica visual: screenshot em 375 e 1440 e comparar com o design.md antes de declarar pronto.

### Fase 5 - Gate de deploy
- Crítico em performance, a11y ou congruência → **bloqueia**.
- **Modo cinematográfico** (aceite registrado no design.md): performance crítica vira **aprovado com ressalvas declaradas** (números medidos x orçamento do design.md). A11y, reduced-motion, fallback sem WebGL/JS, poster como LCP e congruência continuam **bloqueando**.
- Vários altos em slop → **bloqueia** até domar os 3 piores.
- Só médios/baixos → **aprovado com ressalvas** (listar).
- Tudo verde → **aprovado**.

Resumo final sempre: Design Read, dials, preset/direção, uma linha por auditoria com veredicto, slots de imagem pendentes.

## Red Flags - STOP

- "Vou direto pro código" → pulou Design Read e os 2 passes.
- "Creme + serif fica elegante" / "gradiente roxo é seguro" → default de calibração. Justificar por escrito ou trocar.
- "O preset manda max-width / 3 colunas / truncate" → regra da casa vence o preset.
- "Vou reinterpretar o Claude Design, fica melhor" → porte fiel.
- "No redesign aproveito e troco a URL / os campos do form" → proibido sem ok.
- "Coloco uns números pra encher" → número fake é inviolável.
- "Rodei o Impeccable" sem ter rodado → dizer o que rodou de fato.
- "Subo agora, audito depois" → QA é antes.
- "Cliente vai adorar 3D" sem pedido nem aceite no design.md → modo cinematográfico não entra.
- "Gero o vídeo no Higgsfield rapidinho" → crédito pago só com ok e custo estimado antes.

## Excuse | Reality

| Desculpa | Realidade |
|---|---|
| "Uma skill de design já resolve" | Cada uma cobre uma fase. O ganho é o encadeamento. |
| "Design é subjetivo" | Contagem de eyebrows, contraste, CLS e congruência são objetivos. |
| "O motor sugeriu, então tá certo" | O motor é BM25 por palavra-chave. Filtrar pela casa e pelo brief. |
| "Tá bonito na minha tela" | Mobile-first: 375px primeiro. |
| "O preset é assim" | Preset é ponto de partida, não receita. |

## Regras invioláveis

1. **Casa vence tudo** que foi absorvido (presets, dials, motor, taste-skill).
2. **QA roda sempre**, em build, refine ou audit.
3. **Nunca alegar que rodou skill não rodada**; usar fallback e dizer.
4. **Mobile-first.**
5. **Zero número fake**; some quando 0.
6. **Sem travessão** em texto user-facing.
7. **Metadata própria da página.**
8. **Nunca sacrificar tracking/UTM/conversão por score.**
9. **CTA no padrão da casa**: CAIXA ALTA + shimmer, centralizado, teto ~460px, nunca 2 linhas.
10. **Chamar a skill quando ela existe; usar o conhecimento destilado das references para o resto.** Não copiar lógica que a skill orquestrada já executa melhor.
11. **Porte de Claude Design/Figma é fiel.**
12. **Um resumo consolidado sempre**, mesmo tudo verde.

## Quando NÃO usar

- Backend/API/schema sem impacto visual.
- Usuário pediu uma skill só → chamar direto.
- Bug funcional → `superpowers:systematic-debugging`.
- Segurança/prontidão → `production-audit` / `seguranca`.

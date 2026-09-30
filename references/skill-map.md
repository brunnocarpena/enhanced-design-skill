# Mapa de skills — o que o /enhanced-design orquestra

Baseado na thread do @nett0eth "As 5 melhores Skills de Web Design pra Claude" + as skills que o Brunno já tem. Consulte quando precisar decidir qual skill chamar, ou quando o usuário perguntar o que instalar.

## As 5 da thread

### 1. Frontend Design — a oficial da Anthropic
- **Papel:** direção estética; ensina o que **não** fazer (fontes batidas, esquema de cor de sempre, sombra de card clichê). Aula de bom gosto embutida no modelo.
- **Quando:** começar projeto do zero e fugir do default na primeira tentativa. É a base.
- **Local:** `frontend-design` (instalada). Invocar via `Skill: frontend-design`.
- **~277 mil instalações.** Dá pra pedir "só use Geist/Google Sans/DM Sans" que ele ajusta.

### 2. Impeccable — a caixa de ferramentas (Paul Bakaus)
- **Papel:** `/impeccable` + ~20 subcomandos. `animate` (animações coesas), `quieter` (auditoria de "barulho": gradiente demais, peso de fonte pesado, emoji espalhado → enxuga o excesso), `audit` (qualidade + acessibilidade), `document` (gera um `design.md` do design system).
- **Diferencial:** 46 regras determinísticas de detecção de slop (rodam sem depender do modelo adivinhar). Tem CLI para CI (`npx impeccable detect`) e extensão de Chrome.
- **Quando:** iterar em cima do que já existe. "Tá exagerado? quieter. Tá sem vida? animate."
- **Local:** INSTALADA em `~/.claude/skills/impeccable` (05/09/2026, via `npx skills add pbakaus/impeccable -g -a claude-code -s impeccable --copy`). Invocar `Skill: impeccable`. Site: impeccable.style.
- **Fallback no /enhanced-design:** [slop-rules.md](slop-rules.md).

### 3. UIUX Pro Max — a que força o Claude a pensar
- **Papel:** banco de dados de design "com esteroide". 67 estilos de UI, 161 paletas, 57 pares de fonte, 99 diretrizes de UX, 16 tech stacks. Força o modelo a **raciocinar antes de escrever** — busca em 5 domínios em paralelo (produto, estilo, cor, landing, tipografia) e devolve um design system completo com anti-padrões marcados.
- **Quando:** você tem clareza do conteúdo e quer profundidade de design. Quanto mais específico o brief, melhor — ela amplifica sua direção, não lê sua mente.
- **Local:** `ui-ux-pro-max` (instalada). No /enhanced-design o **motor Python é chamado direto** (`scripts/search.py`, protocolo em [ui-ux-pro-max-engine.md](ui-ux-pro-max-engine.md)); `Skill: ui-ux-pro-max` só quando o usuário quer a skill inteira. ~105 mil stars.

### 4. Web Design Guidelines — a auditora (Vercel Labs)
- **Papel:** o QA antes de subir. Audita o código contra 100+ web interface guidelines da Vercel: foco de teclado funcionando, nunca desabilitar zoom, não bloquear paste, indicador de loading em botão, hierarquia de heading, ARIA. Pode anotar os problemas direto no preview (em laranja, onde falta aria-label, onde a navegação por teclado quebra).
- **Quando:** o design já tá bom e você quer garantir que é acessível e não constrange quem usa teclado/leitor de tela.
- **Local:** INSTALADA em `~/.claude/skills/web-design-guidelines` (05/09/2026, via `npx skills add vercel-labs/agent-skills -g -a claude-code -s web-design-guidelines --copy`). Invocar `Skill: web-design-guidelines`.
- **Fallback no /enhanced-design:** [web-interface-guidelines.md](web-interface-guidelines.md).

### 5. Astryx — o design system feito pra agente (Meta)
- **Papel:** não é skill de gosto, é um **design system inteiro**. Cresceu 8 anos dentro da Meta, roda em 13 mil apps, agora open source e agent-ready. 150+ componentes, 7 temas, dark mode, uma CLI que a pessoa e o agente usam do mesmo jeito, e um `CLAUDE.md` no repo pra o Claude entender a casa antes da primeira linha.
- **Quando:** você quer consistência de sistema, não só uma tela bonita. As skills afinam o gosto; o Astryx garante que a fundação já é sólida antes de qualquer prompt. Os dois se somam.
- **Local:** NÃO instalado. React + StyleX, em beta, ~8,2 mil stars.
- **Uso no /enhanced-design:** se o projeto adota o Astryx (ou outro DS estabelecido), use-o como fundação na fase 1.5 em vez de inventar tokens. Não force — ofereça.

## Absorvida sem instalar: taste-skill (Leonxlnx)

- **Origem:** github.com/Leonxlnx/taste-skill, auditada no commit 87da8a4 (29/09/2026), licença MIT. Segura, mas **não instalada**: o conhecimento foi destilado para as references e filtrado pelas regras da casa.
- **O que entrou:** Design Read, dials VARIANCE/MOTION/DENSITY e presets por tipo de página, os 2 passes (plano de tokens + crítica) e os defaults de calibração → [aesthetic-direction.md](aesthetic-direction.md); variantes soft/minimalist/brutalist/stitch → [style-presets.md](style-presets.md); redesign-skill → [redesign-protocol.md](redesign-protocol.md); image-to-code e imagegen web/mobile → [image-to-code.md](image-to-code.md); brandkit → [brandkit.md](brandkit.md); formato DESIGN.md do Stitch → [design-md-template.md](design-md-template.md); motion → [motion-patterns.md](motion-patterns.md) (depois alinhado ao Emil, ver abaixo); checagens mecânicas de slop → [slop-rules.md](slop-rules.md).
- **O que ficou de fora (conflito com a casa ou risco):** max-width de página, colunas travadas por breakpoint, truncate, card com borda lateral, número inventado para "encher", CTA em estilo próprio, RNG/variância obrigatória, `overflow-x-hidden` global, `npx shadcn@latest` e instalação sem versão, hotlink de CDN e picsum, dependência do Stitch, geração de imagem sem aprovar custo, proibição de Lucide/serif contra a marca, `transition: all`, números sempre em mono, `grid-flow-dense`, floating label, janela fake de macOS.

## Absorvidas sem instalar: motion e modo cinematográfico (30/09/2026)

Clonadas, auditadas (sem injeção de prompt) e reescritas com palavras próprias; nenhuma instalada.

- **emilkowalski/skills** @d16ebe6, MIT → [motion-craft.md](motion-craft.md): portão "animar ou não", tokens de easing e duração, física, receitas, checklist de review, vocabulário, performance. Alinhou [motion-patterns.md](motion-patterns.md) (hover com `pointer: fine`, reduced-motion em toda animação, clip-path nas tabs, string `transform` no Motion).
- **greensock/gsap-skills** @aed9cfd, MIT → [gsap-scroll.md](gsap-scroll.md): licença (tudo gratuito), setup vanilla/React com `useGSAP`, core, timeline, ScrollTrigger, plugins, 6 receitas. Corrigido o scroll horizontal do exemplo oficial (`x: () => -dist()` + `invalidateOnRefresh`).
- **CloudAI-X/threejs-skills** @b1c6230 (sem licença), **robbertvanempel/scroll-world-skill** @21da15c (sem licença), **zubair-trabzada/scroll-cinematic-claude** @5f3cc0e (sem licença), **amirmushichge/cinematic-scroll-prompt-kit** @39ac222 (MIT) → [cinematic-mode.md](cinematic-mode.md): ativação por aceite, matriz de 6 motores, pipeline de frames, scroll-world, Three.js/R3F, Spline, brief 2.5D, 10 guarda-corpos. Das sem licença entrou só conhecimento, nenhum trecho copiado.
- **O que ficou de fora:** scripts e engine do zubair e do scroll-world (escrever o nosso), ffmpeg baixado de site de terceiro (só `brew install ffmpeg`), Lenis/unpkg sem SRI, decoders Draco/Basis de CDN, ID opaco de modelo do Higgsfield e qualquer geração paga sem ok, ScrollSmoother/Observer por padrão (scroll-jacking), `quickTo` como cursor customizado, `gsap.context` como padrão em React.

## Frontend-design (plugin)

A versão do plugin (`frontend-design:frontend-design`) é a canônica. Os princípios de direção (grounding, ênfase na mesma família, restrição, um momento de motion, escrita de interface) e os tells de calibração foram destilados em [aesthetic-direction.md](aesthetic-direction.md) e [slop-rules.md](slop-rules.md); invocar a skill quando a direção pede mais profundidade.

## As que o Brunno já tem além da thread

- **seek-patterns** — replica os acabamentos consolidados dos projetos anteriores do Brunno (catálogo em `~/.claude/skills/seek-patterns/CATALOG.md`). Equivalente pessoal de um design system. Fase 2.
- **performance-audit** — PageSpeed/Lighthouse/Core Web Vitals, preservando layout/tracking/UTM/conversão. Fase 4a.
- **congruence** — audita se copy/docs/claims batem com o que o código faz. Fase 4d.
- **production-audit** / **seguranca** — segurança e prontidão de produção. Fora do escopo visual, mas bom antes de deploy sério.

## Diretórios pra achar mais skills (citados na thread)

- **UI Skills** — focado em design engineer, serve pra web design; lista várias e traz coisas de acessibilidade e Shadcn.
- **Type UI** — pra quem pensa visual primeiro; rola e vê preview de cada estética (minimalista, neo-brutalismo, corporativo sofisticado) e chama a que quer.
- **vercel-labs/agent-skills** — coleção oficial de agent skills da Vercel (onde vive o Web Design Guidelines).

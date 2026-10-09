---
name: enhanced-design
description: Use sempre que for CRIAR, REDESENHAR ou fazer QA de qualquer interface web/mobile (landing page, página de captura ou de vendas, dashboard, app, componente, formulário) e o resultado não pode sair genérico. TRIGGER quando o usuário diz "enhanced design", "design caprichado", "roda tudo de design", "página premium", "não quero genérico", "cara de IA", "taste", "redesign", "refazer o visual", "polir a UI", "deixa lindo", "web design", "landing bonita", "brandkit", "logo", "transformar print/imagem em código", "animação", "micro-interação", "scroll", "GSAP", "site cinematográfico", "3D", "WebGL", "Three.js", "Spline", "tipo Apple", "tá cortado", "ficou por cima", "quebrou no celular", "arruma esse print"; quando a sessão cria ou edita hero, seções, componentes, formulários, temas ou tipografia; ANTES de qualquer deploy de página pública. SKIP em mudança só de backend/API/schema sem impacto visual, ou quando o usuário pede explicitamente UMA skill específica ("roda só o performance-audit").
argument-hint: [escopo-ou-url?] [--mode=build|refine|audit] [--target=mobile|desktop|both]
allowed-tools: Read, Grep, Glob, Skill, Task, AskUserQuestion, WebFetch, Bash(python3:*), Bash(cd:*), Bash(git diff:*), Bash(git status:*), Bash(ls:*), Bash(find:*), Bash(wc:*)
---

# Enhanced Design

O **maestro de web design**. Encadeia o pipeline que um estúdio seguiria: **abrir um briefing curto**, ler o brief, decidir a direção, montar o sistema, aplicar o acabamento da casa, construir e só subir depois de auditar. O briefing decide o **nível de impacto** (Enxuto/Equilibrado/Cinematográfico) e é ele que carrega — ou deixa de fora — as sub-skills pesadas (motion, GSAP, 3D). O padrão é a contenção. Orquestra as skills instaladas (ui-ux-pro-max, frontend-design, seek-patterns, performance-audit, impeccable, web-design-guidelines, congruence) e carrega, em `references/`, o conhecimento destilado da taste-skill, do motor do ui-ux-pro-max, do frontend-design, do Emil Kowalski (motion), das skills oficiais do GSAP e de fontes de 3D/scrollytelling (modo cinematográfico).

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
| Corrigir defeito, print do cliente, "tá cortado/por cima/quebrou no celular" | [fix-requests.md](references/fix-requests.md) |
| Tem print, Claude Design, Figma ou vai gerar referência | [image-to-code.md](references/image-to-code.md) |
| Motion — só nível Equilibrado+ (defaults e proibições) | [motion-patterns.md](references/motion-patterns.md) |
| Micro-interação — só nível Equilibrado+ | [motion-craft.md](references/motion-craft.md) |
| Scroll narrativo, pin, scrub, SplitText, SVG — **só nível Cinematográfico** | [gsap-scroll.md](references/gsap-scroll.md) |
| 3D, WebGL, frames, scroll-world — **só nível Cinematográfico** | [cinematic-mode.md](references/cinematic-mode.md) |
| Fase 2 | [house-rules.md](references/house-rules.md) |
| Fase 4b / 4c | [web-interface-guidelines.md](references/web-interface-guidelines.md) / [slop-rules.md](references/slop-rules.md) |
| Motor falhou ou skill faltando | [reasoning-fallback.md](references/reasoning-fallback.md) |
| Origem de cada peça | [skill-map.md](references/skill-map.md) |

Ler só a referência da fase atual, não todas de uma vez.

## Modos

- **build**: interface nova. Fases 0 → 5 completas.
- **refine**: melhorar o que existe. Fase 0 → [redesign-protocol.md](references/redesign-protocol.md) → Fase 2 → 4 → 5. Nunca reescrever nem migrar stack.
- **audit**: só Fase 4 + gate. Não redesenha. Print com defeito → auditoria por impacto de [fix-requests.md](references/fix-requests.md) seção 2.

Em refine e audit, toda correção de layout leva os blocos **Preserve / Não use / Validação** ([fix-requests.md](references/fix-requests.md) seção 3) e um defeito de layout por vez.

## Funil de impacto

**Base fixa — fora do funil, entra sempre, em todos os níveis.** As camadas de proteção não se negociam no briefing e nunca são removidas por "nível mais baixo": anti-slop ([slop-rules.md](references/slop-rules.md) + impeccable), regras da casa e de comunicação ([house-rules.md](references/house-rules.md) + RULES-GLOBAL), não-quebrar o que já está certo ([fix-requests.md](references/fix-requests.md): Preserve/Não use/Validação), direção/sistema (Design Read + dials + motor) e o QA completo da Fase 4. **Composição e estrutura também são base fixa**, não "nível de impacto": largura total sem zona morta, hierarquia visual clara, preencher o espaço com propósito, sem bloco estreito flutuando num canvas vazio, sem borda de um lado, sem card/hero solto. Isso existe para evitar problema e evitar estrago — é piso, não escolha.

**Enxuto NÃO é "sem design". Enxuto é editorial limpo e impecável (tipo Apple): composição forte, hierarquia clara, imagem bem usada, espaço preenchido.** O que o nível baixo tira é EFEITO (3D, GSAP, motion pesado, gradiente empilhado), nunca capricho de composição. Se o resultado ficou flat, vazio, com faixa morta nas laterais, hero solto ou cara de template de IA, **falhou a base — não é o que Enxuto significa**: voltar e dar estrutura (hero com âncora visual que preenche a dobra, grade que ocupa a largura, foto full-bleed ou split em vez de miniatura centralizada no vazio).

O que o briefing decide é **só o nível de impacto**: cinematográfico ou simples. A resposta de **Impacto** (Fase 0) é o único gatilho que carrega ou deixa de fora as sub-skills pesadas de efeito. O padrão é a contenção: nada pesado entra sem ser pedido. É isto que impede o maestro de jogar 3D, GSAP e gradiente empilhado numa página que só precisava ser limpa — e, do outro lado, a base de composição impede que "limpa" vire "pobre e vazia".

| Nível | MOTION dial | Acrescenta sobre a base | NÃO entra |
|---|---|---|---|
| **Enxuto** (padrão) | 0-1 (none/fade sutil) | nada além da base | motion-craft, gsap-scroll, cinematic-mode |
| **Equilibrado** | 2-4 | + motion-patterns, motion-craft (micro-interações sutis, 1 detalhe) | gsap-scroll pesado, cinematic-mode |
| **Cinematográfico** | 5-10 | + gsap-scroll, cinematic-mode (3D/WebGL/scroll-world) | — |

Na dúvida, o nível é o **de baixo**. Subir exige resposta explícita no briefing, nunca "o cliente vai gostar". A base fixa acima vale igual nos três níveis.

## Workflow

### Fase 0 - Enquadrar e briefing
1. Ler o escopo (URL, arquivos, `git status`/`git diff`) e contar os entregáveis pedidos (quantas páginas, seções, telas). Entregar todos; nada de "exemplo de uma e o resto igual".
2. Detectar o ponto de partida: existe Claude Design/Figma (→ porte fiel, [image-to-code.md](references/image-to-code.md) seção 1-3), site existente (→ refine), ou greenfield (→ build).
3. **Briefing interativo (`AskUserQuestion`), SEMPRE em build/refine antes de qualquer código.** Auto-preencher o que as etapas 1-2 já revelam e perguntar só o que falta, numa **única chamada** (até 4 perguntas). O **nível de impacto é pergunta obrigatória e nunca se presume** — é ele que abre ou fecha as sub-skills (ver [Funil de impacto](#funil-de-impacto)):
   - **Impacto** (escolha única, obrigatória): `Enxuto` (limpo, rápido, sem efeito — recomendado) · `Equilibrado` (micro-interações sutis, 1 destaque) · `Cinematográfico` (3D/scroll-world/GSAP; troca performance por impacto).
   - **Prioridade** (múltipla): Conversão · Impressão de marca · Velocidade/SEO · Acessibilidade.
   - **Tipo** (só se não estiver claro): Página/LP · Site/várias páginas · Seção/componente · Redesign.
   - **Identidade** (só se a etapa 2 não resolveu): Claude Design/Figma · Marca/brandkit · Partir do zero.
   Sem resposta / pulou → **Enxuto + Conversão**. Registrar as respostas no `design.md`. Em `audit` não há briefing (não redesenha).
4. Escrever o **Design Read** em uma linha ([aesthetic-direction.md](references/aesthetic-direction.md) seção 1): "Lendo isto como: ... Trabalho principal da página: ..." e fixar target mobile-first (desktop é complemento).

### Fase 1 - Direção e sistema (build; refine só em overhaul)
5. Fixar os **dials** VARIANCE / MOTION / DENSITY. O **MOTION vem travado pelo nível de impacto do briefing** (Enxuto 0-1, Equilibrado 2-4, Cinematográfico 5-10); VARIANCE e DENSITY pelo preset do tipo de página. Justificar ([aesthetic-direction.md](references/aesthetic-direction.md) seção 2).
6. Rodar o **motor do ui-ux-pro-max** pelo protocolo de [ui-ux-pro-max-engine.md](references/ui-ux-pro-max-engine.md) (`--domain product` → `--design-system` → aprofundar → `--stack` → `--domain ux`). Filtrar a saída pela tabela da casa. Se o usuário quiser a skill inteira, `Skill: ui-ux-pro-max`.
7. Projeto sem identidade → escolher um preset de [style-presets.md](references/style-presets.md) ou declarar "sem preset", e justificar. Brief pede marca → [brandkit.md](references/brandkit.md) antes de qualquer tela.
8. **Passe 1**: plano de tokens (4-6 hex, 1-2 famílias com papel, wireframe ASCII, 2-3 princípios). **Passe 2**: criticar o plano contra o brief e contra os defaults de calibração; dizer o que mudou. Só então codar. Para direção estética mais funda, `Skill: frontend-design:frontend-design`.
9. Registrar no `design.md` da raiz do projeto (greenfield: na pasta onde o projeto vai nascer; teste sem projeto: scratchpad) no formato de [design-md-template.md](references/design-md-template.md) (atualizar se já existe). Um DS por projeto: se já existe um (shadcn, Astryx, o da marca), ele é a fundação.

### Fase 2 - Padrões da casa
10. `Skill: seek-patterns` (fallback: [house-rules.md](references/house-rules.md)). CTA CAIXA ALTA + shimmer, grade auto-fit, sem max-width de página, padding de seção único, primeira dobra visível, fotos distintas, ícone oficial do WhatsApp, UX de formulário, ícone em cima do texto no mobile.

### Fase 3 - Construção
11. Implementar seguindo o design.md. Motion **pelo nível do briefing**: Enxuto = sem motion ou fade sutil; Equilibrado = micro-interações por [motion-craft.md](references/motion-craft.md)/[motion-patterns.md](references/motion-patterns.md); Cinematográfico = scroll narrativo por [gsap-scroll.md](references/gsap-scroll.md) e 3D/WebGL por [cinematic-mode.md](references/cinematic-mode.md). Não subir de nível no meio da construção. **Zero número fake**, **sem travessão** em texto user-facing, **metadata própria** (title = H1, description = sub + data, og:image da própria página).
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
- **Nível Cinematográfico** (escolhido no briefing, registrado no design.md): performance crítica vira **aprovado com ressalvas declaradas** (números medidos x orçamento do design.md). A11y, reduced-motion, fallback sem WebGL/JS, poster como LCP e congruência continuam **bloqueando**.
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
- "Cliente vai adorar 3D" sem o nível Cinematográfico escolhido no briefing → não entra. Na dúvida, o nível é o de baixo.
- "Pulo o briefing, já sei o que fazer" → o briefing (nível de impacto) é obrigatório em build/refine; é ele que impede o excesso.
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
13. **Briefing antes do código** em build/refine; o nível de impacto nunca se presume e o padrão é o Enxuto.

## Quando NÃO usar

- Backend/API/schema sem impacto visual.
- Usuário pediu uma skill só → chamar direto.
- Bug funcional → `superpowers:systematic-debugging`.
- Segurança/prontidão → `production-audit` / `seguranca`.

---
name: enhanced-design
description: Use sempre que for CRIAR, REDESENHAR ou fazer QA de qualquer interface web/mobile (landing page, dashboard, app, página de captura, componente) e quiser o resultado no padrão máximo — não uma tela genérica. É o MAESTRO de design: um comando só que roda o pipeline inteiro (raciocinar → montar design system → aplicar os padrões da casa → construir → auditar performance/acessibilidade/slop/congruência → decidir deploy), orquestrando as melhores skills de web design (frontend-design, ui-ux-pro-max, seek-patterns, performance-audit, congruence). TRIGGER quando o usuário menciona "enhanced design", "design caprichado", "roda tudo de design", "página premium", "não quero genérico", "redesign", "polir a UI", "deixa lindo", "web design", "landing bonita"; quando a sessão cria/edita hero, seções, componentes, formulários, temas, tipografia; ANTES de qualquer deploy de página pública. Faz mais do que uma skill isolada — encadeia todas. SKIP em mudança puramente de backend/API/schema sem impacto visual, ou quando o usuário pede explicitamente só UMA skill específica (ex: "roda só o performance-audit").
argument-hint: [escopo-ou-url?] [--mode=build|refine|audit] [--target=mobile|desktop|both]
allowed-tools: Read, Grep, Glob, Skill, Task, WebFetch, Bash(git diff:*), Bash(git status:*), Bash(ls:*), Bash(find:*), Bash(wc:*)
---

# Enhanced Design

O **maestro de web design**. Uma skill isolada afina uma coisa; esta encadeia o pipeline inteiro, na ordem que um estúdio de verdade seguiria: primeiro pensa, depois desenha o sistema, aplica o acabamento da casa, constrói, e só sobe depois de auditar. Não é para produzir "mais uma tela" — é para produzir uma interface que ninguém confunde com template.

Ela **não reimplementa** o que outras skills já fazem bem. Ela **orquestra**: chama cada skill no momento certo, passa o contexto adiante, e fecha com um gate de deploy. Inspirada na thread do @nett0eth ("As 5 melhores Skills de Web Design pra Claude") e nas skills que você já tem instaladas.

## Princípio central

```
DESIGN BOM NÃO É SORTE DO MODELO A CADA COMPONENTE.
É UM SISTEMA QUE JÁ NASCE INTENCIONAL, CONSISTENTE E ACESSÍVEL —
E UM QA QUE NÃO DEIXA SUBIR NADA GENÉRICO, LENTO OU INCONGRUENTE.
```

A sacada da thread vale aqui: em vez de torcer para o modelo ter bom senso a cada botão, você entrega um sistema onde tudo já nasce sólido — e no fim passa por uma bateria de auditorias antes de subir. Raciocínio na frente, construção no meio, QA no fim. As skills se **somam**, não competem.

## O que esta skill orquestra

Cada fase chama uma skill real. Antes de chamar, **verifique se está instalada** (aparece na lista de skills disponíveis). Se estiver, invoque via tool `Skill`. Se não, use o **fallback inline** apontado (um checklist que captura a essência da skill que falta) e avise o usuário que instalar a skill original melhora o resultado.

| Fase | Skill (papel na thread) | Instalada? → ação | Fallback se faltar |
|------|-------------------------|-------------------|--------------------|
| 1. Raciocínio + design system | `ui-ux-pro-max` (força o Claude a pensar: estilos, paletas, fontes, 99 diretrizes UX, stacks) | invocar `Skill: ui-ux-pro-max` | [references/reasoning-fallback.md](references/reasoning-fallback.md) |
| 1. Direção estética | `frontend-design` (a oficial da Anthropic: foge do templated, ensina o que NÃO fazer) | invocar `Skill: frontend-design` | [references/reasoning-fallback.md](references/reasoning-fallback.md) |
| 2. Padrões da casa | `seek-patterns` (replica os acabamentos dos seus projetos anteriores) | invocar `Skill: seek-patterns` | [references/house-rules.md](references/house-rules.md) |
| 3. Construção | (implementação seguindo o sistema das fases 1-2) | — | [references/house-rules.md](references/house-rules.md) |
| 4a. QA performance | `performance-audit` (PageSpeed/Core Web Vitals sem quebrar tracking/conversão) | invocar `Skill: performance-audit` | — |
| 4b. QA acessibilidade/interface | Web Design Guidelines (Vercel Labs — 100+ regras) | raramente instalada | [references/web-interface-guidelines.md](references/web-interface-guidelines.md) |
| 4c. QA anti-slop | Impeccable (Paul Bakaus — 46 regras de "barulho" de IA) | raramente instalada | [references/slop-rules.md](references/slop-rules.md) |
| 4d. QA congruência | `congruence` (o que a copy promete bate com o que o código faz) | invocar `Skill: congruence` | — |

**Design system pronto (fase 1.5, opcional):** se o projeto usa/quer o Astryx (design system open-source da Meta, React + StyleX, agent-ready) ou outro DS já estabelecido, use-o como fundação em vez de inventar tokens do zero. Aponte isso ao usuário como opção — não force.

Detalhe do mapeamento completo (o que cada skill faz, links, stars, como instalar as que faltam): [references/skill-map.md](references/skill-map.md).

## Modos de operação

Identifique o modo no começo. Se ambíguo, pergunte **uma vez**.

### `build` — interface nova, do zero
Rodar o pipeline completo: fases 1 → 2 → 3 → 4. O maior ganho está aqui — a fundação nasce intencional.

### `refine` — melhorar interface existente
Pular a fundação pesada. Focar em: `seek-patterns` (o que falta do padrão da casa), anti-slop (domar exagero, dar vida onde está morto), e as auditorias 4a/4b/4d. É o "peguei uma página pronta e quero domar/elevar".

### `audit` — só QA antes de subir
Rodar apenas a fase 4 (4a→4b→4c→4d) + a decisão de deploy. Não redesenha; carimba (ou barra) o que já existe.

## Workflow

### Fase 0 — Enquadrar
1. Ler o escopo (URL, arquivos tocados na sessão, ou descrição). `git status`/`git diff` ajudam a ver o que mudou.
2. Definir **modo** (build/refine/audit) e **target** (mobile-first sempre; desktop é complemento — regra da casa).
3. Definir o **assunto** concreto: que produto, que público, que sensação. Sem isso, design vira decoração. As skills de design **amplificam sua direção, não leem sua mente** — quanto mais específico o brief, melhor o retorno.

### Fase 1 — Raciocínio + design system (modo build/refine)
4. Invocar `ui-ux-pro-max` para o modelo **raciocinar antes de escrever**: escolher estilo, paleta, par de fontes, diretrizes de UX e stack. Saída = um design system com anti-padrões já marcados.
5. Invocar `frontend-design` para a **direção estética distinta**: escolhas opinativas de tipografia/layout/cor específicas do brief, e evitar o cheiro de template (fontes batidas, gradiente roxo, sombra de card clichê, emoji espalhado).
6. Consolidar em um **design system enxuto** (tokens: cor, tipografia, espaçamento, raio, sombra, movimento). Registrar num `design.md` do projeto se ele durar mais que a sessão.

### Fase 2 — Padrões da casa
7. Invocar `seek-patterns`: aplicar os acabamentos já consolidados dos projetos do Brunno (CTA em CAIXA ALTA + shimmer, largura de CTA centralizada, espaçamento vertical consistente entre seções, primeira dobra visível, sem palavra órfã, foto da pessoa distinta por seção, ícone oficial do WhatsApp, UX de formulário — olhinho na senha, erro inline, estado de envio). Fonte: `references/house-rules.md` se a skill não estiver disponível.

### Fase 3 — Construção
8. Implementar seguindo o design system das fases 1-2 e os padrões da fase 2. **Zero número fake/hardcoded** na UI — tudo dinâmico, some quando 0. **Sem em-dash** em texto user-facing (use hífen). **Metadata própria da página** (title = H1, description = subtítulo + data; nunca herdar de outra campanha).
9. Se estiver criando página nova, montar a metadata a partir da própria página (title/description/og:image), não reaproveitar de outra.

### Fase 4 — QA antes de subir (SEMPRE, em qualquer modo)
Rodar na ordem; cada uma pode barrar o deploy:

- **4a — Performance:** invocar `performance-audit`. Primeira dobra rápida, LCP/CLS/INP no verde, sem quebrar tracking/UTM/conversão.
- **4b — Acessibilidade/interface:** rodar o checklist de [references/web-interface-guidelines.md](references/web-interface-guidelines.md) (foco de teclado visível, ARIA, não desabilitar zoom, não bloquear paste, hierarquia de heading, indicador de loading em botão). Se a skill Web Design Guidelines estiver instalada, invoque-a.
- **4c — Anti-slop:** rodar o checklist de [references/slop-rules.md](references/slop-rules.md) (gradiente demais, peso de fonte pesado em todo heading, borda de acento clichê, emoji decorativo, texto genérico "Bem-vindo à nossa plataforma"). Se Impeccable estiver instalada, invoque-a.
- **4d — Congruência:** invocar `congruence`. A copy/CTA/metadata promete só o que o código entrega.

### Fase 5 — Decisão de deploy
Consolidar os veredictos das auditorias num gate único (ver abaixo).

## Decisão pré-deploy (gate)

Junte os resultados das 4 auditorias. Regra:

- Qualquer item **crítico** em performance, acessibilidade ou congruência → **bloqueia deploy**.
- Múltiplos **altos** em slop/anti-padrão → **bloqueia** até domar os 3 piores.
- Só **médios/baixos** → **aprovado com ressalvas** (listar as ressalvas).
- Tudo verde + Core Web Vitals bom + congruente → **aprovado**.

Emitir sempre um resumo final consolidado, mesmo se tudo verde, com uma linha por auditoria e o veredicto de cada uma.

## Red Flags — STOP

Se você se pegar pensando ou fazendo:

- "Vou direto pro código, o design eu improviso na hora" → pulou a fase 1, resultado vira genérico.
- "Uso o mesmo hero/gradiente/fonte de sempre" → é exatamente o cheiro de template que a fase 1 existe pra evitar.
- "Rodei uma skill de design, tá bom" → o valor está no **encadeamento**; QA (fase 4) não é opcional.
- "Vou dizer que rodei o Impeccable/Web Design Guidelines" sem eles instalados → **mentira/incongruência**. Use o fallback e diga que usou o fallback.
- "Subo agora, audito depois" → auditoria é antes de subir, não depois.
- "Boto uns números redondos na UI pra ficar cheio" → número fake é regra inviolável contra. Só dado real.
- "Score/estilo bonito com formulário quebrado ou copy mentindo" → regressão, não vitória.

→ PARE. Volte à fase correspondente.

## Excuse | Reality

| Desculpa | Realidade |
|----------|-----------|
| "Uma skill de design já resolve" | Cada uma cobre uma fase. O ganho é o pipeline inteiro somado. |
| "Design é subjetivo, não dá pra auditar" | Slop, acessibilidade, performance e congruência são objetivos. Audite. |
| "Não tenho o Impeccable/Vercel instalado, então pulo o QA" | Tem os fallbacks inline. QA não é opcional. |
| "Tá bonito na minha tela" | Na sua tela desktop. Regra da casa: mobile-first, sempre. |
| "A copy vende, o código a gente ajusta" | Copy que promete o que o código não faz = incongruência que quebra confiança. |
| "Já é o padrão dos outros projetos, confia" | `seek-patterns`/house-rules existem pra provar, não pra confiar. |

## Regras invioláveis

1. **QA (fase 4) roda sempre** — build, refine ou audit. Design sem auditoria não sobe.
2. **Nunca alegar que rodou uma skill não instalada** — use o fallback e diga que usou o fallback.
3. **Mobile-first é default** — desktop é complemento.
4. **Zero número fake/hardcoded na UI** — só dado real; some quando 0.
5. **Sem em-dash em texto user-facing** — hífen "-", varrer antes de subir.
6. **Metadata é própria da página** — title=H1, description=sub+data; nunca herdar de outra campanha.
7. **Nunca sacrificar tracking/UTM/conversão por score** — performance preserva conversão.
8. **CTA no padrão da casa** — CAIXA ALTA + shimmer, centralizado, largura total (teto ~460px), nunca 2 linhas.
9. **Um resumo consolidado de deploy sempre** — mesmo tudo verde.
10. **Não reimplementar o que a skill orquestrada já faz** — chamar a skill, não copiar a lógica dela.

## Quando NÃO usar

- Mudança puramente de backend/API/schema sem impacto visual → design não se aplica.
- Usuário pediu explicitamente **uma** skill ("roda só o performance-audit") → chame direto essa, sem o maestro.
- Debug de bug funcional (não visual) → `superpowers:systematic-debugging`.
- Auditoria de segurança/prontidão de produção → `production-audit` / `seguranca`.

## Skills relacionadas (as peças que este maestro rege)

- **frontend-design** — direção estética distinta; a fundação anti-template. Fase 1.
- **ui-ux-pro-max** — banco de design que força raciocínio; estilos/paletas/fontes/UX. Fase 1.
- **seek-patterns** — replica os acabamentos dos seus projetos anteriores. Fase 2.
- **performance-audit** — PageSpeed/Core Web Vitals sem quebrar conversão. Fase 4a.
- **congruence** — copy/docs batem com o código. Fase 4d.
- **production-audit** / **seguranca** — segurança e prontidão; complementares, fora do escopo visual.

## Por que esta skill existe

Cada skill de design isolada é ótima numa coisa e cega no resto. Frontend-design te dá gosto mas não mede performance; ui-ux-pro-max te faz pensar mas não audita acessibilidade; performance-audit acha o gargalo mas não julga se ficou bonito. Na prática, um humano bom faz tudo isso em sequência — pensa, desenha o sistema, aplica o acabamento da casa, constrói, e faz QA antes de entregar. Esta skill codifica essa sequência para que "design bom" deixe de depender de você lembrar de rodar cinco coisas na ordem certa, e passe a ser **um comando**.

---

**Base estrutural:** modelada nas skills `congruence` e `performance-audit` (mesmo formato de frontmatter com TRIGGER/SKIP, princípio central, modos, workflow numerado, red flags, Excuse|Reality, gate pré-deploy e regras invioláveis).

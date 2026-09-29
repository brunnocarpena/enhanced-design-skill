# Interface & acessibilidade — fallback do Web Design Guidelines (Vercel Labs)

Use quando a skill Web Design Guidelines **não** estiver instalada. Captura a essência das 100+ regras de interface da Vercel. Rodar na fase 4b. Para cada falha, apontar onde no código e como corrigir.

> Se a skill estiver instalada (parte de `vercel-labs/agent-skills`), prefira invocá-la. Este checklist é o plano B honesto. Não é auditoria WCAG completa — é o QA de interface pré-deploy.

## Teclado e foco
- [ ] Todo elemento interativo é alcançável por **Tab**, na ordem visual/lógica.
- [ ] **Foco visível** em todo controle (não remover `outline` sem substituir por um `:focus-visible` claro).
- [ ] Nenhuma armadilha de foco (modal fecha com Esc e devolve o foco ao gatilho).
- [ ] CTA principal recebe foco e dá pra ativar com Enter/Espaço.

## Zoom, seleção e input (nunca hostilizar o usuário)
- [ ] **Nunca desabilitar zoom** (`user-scalable=no`, `maximum-scale=1` são proibidos).
- [ ] **Não bloquear paste** em campos (senha, e-mail, cupom) — deixar colar.
- [ ] Não desabilitar seleção de texto sem motivo.
- [ ] `inputmode`/`type` corretos (email, tel, number) pro teclado certo no mobile.
- [ ] Fonte de input ≥ 16px no mobile (evita zoom automático do iOS).

## Semântica e ARIA
- [ ] **Hierarquia de heading** correta (um `h1`, sem pular níveis).
- [ ] Ícone-botão sem texto tem `aria-label`.
- [ ] Imagem informativa tem `alt`; imagem decorativa tem `alt=""`.
- [ ] Landmarks (`header`, `nav`, `main`, `footer`) presentes.
- [ ] Estado de componentes expresso (`aria-expanded`, `aria-current`, `aria-invalid`).
- [ ] Link vs botão usados corretamente (navegação = `a`; ação = `button`).

## Feedback e estados
- [ ] **Indicador de loading em botão** durante submit (spinner/estado disabled) — nada de clicar e não acontecer nada.
- [ ] Erro de formulário **inline por campo**, não só um alerta genérico no topo.
- [ ] Estados hover/active/focus/disabled pensados em todo controle.
- [ ] Toque no mobile ≥ 44×44px de alvo.

## Contraste e legibilidade
- [ ] Contraste de texto ≥ 4.5:1 (corpo) / 3:1 (texto grande).
- [ ] Não transmitir informação só por cor (erro tem ícone/texto, não só vermelho).
- [ ] Line-height e medida de linha confortáveis (≈ 45-75 caracteres).

## Movimento
- [ ] Respeitar `prefers-reduced-motion` (reduzir/cortar animações).
- [ ] Nada de auto-play que não pode ser pausado.

## Toque (mobile)
- [ ] 8px de espaço entre alvos de toque vizinhos.
- [ ] `touch-action: manipulation` nos controles; `cursor: pointer` em todo clicável.
- [ ] Feedback visual em até 100ms (pressionado com scale 0.95-0.98).
- [ ] Nenhuma ação que só existe no hover.
- [ ] Sem swipe horizontal no conteúdo principal; `env(safe-area-inset-*)` em barra fixa.

## Performance percebida
- [ ] Imagem com `width`/`height` (ou aspect-ratio), WebP/AVIF, `srcset`, lazy fora da dobra; hero com prioridade.
- [ ] Fonte via `next/font` ou `font-display: swap`; CLS < 0.1.
- [ ] Lista com 50+ itens virtualizada; skeleton só se a espera passar de 300ms.
- [ ] Scripts de terceiros `async`/`defer` (sem quebrar tracking).

## Layout
- [ ] `min-h-dvh`; escala de z-index nomeada (nada de `z-[9999]`).
- [ ] Conteúdo não fica escondido atrás de barra fixa (padding compensando).
- [ ] Sem scroll aninhado; espaçamento em múltiplos de 4/8.
- [ ] Testado em 375, 768, 1024, 1440 e paisagem.

## Tipo e cor
- [ ] Corpo com line-height 1.5-1.75; escala tipográfica fixa; pesos 400/500/600-700.
- [ ] Cor por token semântico (`--cor-erro`, não hex solto); dark mode dessaturado, não invertido.
- [ ] Número em tabela/preço com `tabular-nums`.

## Animação
- [ ] 150-300ms (complexa ≤ 400ms, nunca > 500ms); só transform/opacity.
- [ ] No máximo 1-2 elementos animando por tela; entrada ease-out, saída ease-in mais curta (60-70%).
- [ ] Stagger 30-60ms; animação interrompível; modal anima a partir do gatilho.

## Formulários
- [ ] Label visível sempre (placeholder não é label); label em cima, erro embaixo, 8px de distância.
- [ ] Validar no blur, não a cada tecla; erro diz causa e como resolver.
- [ ] No submit com erro: foco no primeiro campo inválido + resumo se forem vários; `role="alert"`.
- [ ] `autocomplete` correto; input ≥ 44px de altura; texto de ajuda persistente.
- [ ] Multi-etapas com progresso e botão voltar; ação destrutiva com confirmação e desfazer.
- [ ] Disabled com opacidade 0.38-0.5; timeout com "tentar de novo"; rascunho salvo em formulário longo.
- [ ] Toast só para evento passageiro, `aria-live="polite"`, 3-5s.

## Navegação
- [ ] Um CTA primário por tela; item ativo marcado na nav.
- [ ] Deep link funciona; voltar preserva estado e scroll.
- [ ] Skip-link para `<main>`; bottom nav ≤ 5 itens; breadcrumb a partir de 3 níveis.
- [ ] Modal nunca é o fluxo principal; scrim 40-60%.

## Ícones e gráficos
- [ ] Um set de ícones, um stroke; logo de marca oficial; contraste do ícone ≥ 3:1.
- [ ] Gráfico: tipo certo para o dado; tooltip acessível por teclado; tabela alternativa; nunca vermelho/verde sozinhos; estados vazio e erro; número em pt-BR (1.234,56); agregar acima de 1000 pontos.

## Estados (skeleton, vazio, erro)
- [ ] Skeleton com o formato do layout final, não barra genérica.
- [ ] Estado vazio convida à ação (o que fazer agora), não só "Nenhum item".
- [ ] Placeholder, foco e texto de ajuda também passam AA.
- [ ] Botão fantasma sobre foto tem scrim atrás.

## Antes de entregar
- [ ] Zoom de texto 200% sem quebrar; claro e escuro testados separadamente.
- [ ] Nada pula ao pressionar; nada escondido sob barra sticky; mudança de estado anunciada ao leitor de tela.

## Como reportar
Para cada falha: **onde (arquivo:linha/seletor) → regra violada → correção → severidade**. Crítico = quebra uso por teclado/leitor de tela ou hostiliza o usuário (zoom/paste). Alto = falta feedback/estado. Médio/baixo = polimento.

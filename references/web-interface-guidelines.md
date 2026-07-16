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

## Como reportar
Para cada falha: **onde (arquivo:linha/seletor) → regra violada → correção → severidade**. Crítico = quebra uso por teclado/leitor de tela ou hostiliza o usuário (zoom/paste). Alto = falta feedback/estado. Médio/baixo = polimento.

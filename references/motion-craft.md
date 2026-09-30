# Motion craft (Emil Kowalski)

Ler quando o dial MOTION for ≥ 3, ao construir micro-interações (botão, popover, modal, drawer, toast, tabs) e ao revisar qualquer animação. Complementa [motion-patterns.md](motion-patterns.md) (defaults, proibições): aqui ficam valores exatos, receitas e o checklist de review. Fonte: emilkowalski/skills (MIT).

## 1. Animar ou não (o portão vem antes do código)

| Frequência de uso | Decisão |
|---|---|
| 100+ vezes/dia (atalho de teclado, command palette) | Nenhuma animação. Nunca. |
| Dezenas/dia (hover, navegação em lista, toggle frequente) | Tirar ou reduzir ao quase imperceptível |
| Ocasional (modal, drawer, toast, configurações) | Animação padrão |
| Rara / primeira vez (onboarding, sucesso, empty state) | Aqui mora o orçamento de "delight" |

- Ação disparada por teclado nunca anima (Raycast não anima abrir/fechar, e está certo).
- Dar nome ao propósito antes de escrever: **feedback**, **consistência espacial**, **indicação de estado**, **evitar mudança brusca**, **explicação** (só marketing/onboarding) ou **delight** (só no nível raro). Sem nome = não constrói.
- Dado que o usuário lê ou usa (gráfico, tabela, saldo) não se mexe por estilo. Efeito decorativo de cursor é para página de marketing.
- "Não animar" é resultado válido.
- Ferramenta mais barata que resolve: transição CSS (hover, press, toggle por classe) → `@starting-style` (entrada sem JS) → animação CSS (movimento predeterminado que precisa ficar liso com a página carregando) → WAAPI (`el.animate()`, controle por JS sem lib) → Motion (spring, layout, exit, gesto). Não instalar lib para um fade.

## 2. Tokens: easing e duração

```css
:root {
  --ease-out: cubic-bezier(0.23, 1, 0.32, 1);     /* entrada/saída de UI */
  --ease-in-out: cubic-bezier(0.77, 0, 0.175, 1); /* algo já na tela indo de A para B */
  --ease-drawer: cubic-bezier(0.32, 0.72, 0, 1);  /* drawer estilo iOS (Ionic) */
  --dur-press: 160ms; --dur-tooltip: 125ms; --dur-dropdown: 200ms; --dur-modal: 250ms;
}
```

| Situação | Easing |
|---|---|
| Entrando ou saindo | `--ease-out` |
| Movendo/transformando na tela | `--ease-in-out` |
| Hover / troca de cor | `ease` |
| Movimento constante (marquee, barra de progresso, hold-to-confirm) | `linear` |
| Na dúvida | `--ease-out` |

- **Nunca `ease-in` em UI**: atrasa justamente o instante que o olho está olhando. `ease-out` a 200ms parece mais rápido que `ease-in` a 200ms.
- As curvas nativas do CSS são fracas; usar os tokens. Curva nova vem de easing.dev ou easings.co, nunca inventada. Se o projeto já tem `--ease-*`, estender, não criar sistema paralelo.

| Elemento | Duração |
|---|---|
| Feedback de botão pressionado | 100-160ms |
| Tooltip, popover pequeno | 125-200ms |
| Dropdown, select | 150-250ms |
| Modal, drawer | 200-500ms |
| Marketing / explicativo | pode ser mais longo |

Regra: UI fica abaixo de 300ms (180ms num dropdown parece mais responsivo que 400ms).

## 3. Física, origem, entrada/saída, interrupção, stagger

- **Nunca `scale(0)`**: começar de `scale(0.9-0.97)` + `opacity: 0`. Nada no mundo real surge do nada.
- **Pressionado**: `scale(0.97)` no `:active` (faixa aceitável 0.95-0.98). `scale()` escala os filhos junto, e é isso que dá sensação física.
- **Origem**: popover, dropdown, menu e tooltip crescem a partir do gatilho (`transform-origin: var(--transform-origin)` no Base UI/Radix). **Modal é exceção**: fica centralizado.
- **`translate` em %** é relativo ao próprio elemento: `translateY(100%)` esconde qualquer drawer/toast sem medir altura. Preferir a px fixo.
- **Sai pelo mesmo caminho que entrou** (toast que sobe de baixo sai por baixo; painel da direita sai pela direita). É o que torna o swipe-to-dismiss óbvio.
- **Tempo assimétrico**: lento onde o usuário decide (segurar para confirmar: 2s `linear`), rápido onde o sistema responde (soltar: 200ms `--ease-out`). Saída mais rápida que entrada.
- **Interrupção**: o que dispara rápido (toast, toggle, expandir) usa **transition**, não `@keyframes`. Transition retoma do valor atual; keyframe recomeça do zero. Gesto usa **spring**, que carrega a velocidade.
- **Nunca travar input durante a transição**; ao interromper, animar a partir do valor atual na tela, não do valor-alvo (senão pula).
- **Stagger**: 30-80ms entre itens. Decorativo: nunca bloqueia clique enquanto roda. Não usar em lista que o usuário passa o dia rolando.

**Springs (Motion):**
- Recomendado (estilo Apple, mais fácil de raciocinar): `{ type: "spring", duration: 0.5, bounce: 0.2 }`.
- Física clássica: `{ type: "spring", mass: 1, stiffness: 100, damping: 10 }` (é o valor do exemplo de rastrear o mouse com `useSpring`, efeito decorativo).
- Bounce sutil (0.1-0.3) e só em drag-to-dismiss ou interação lúdica. Padrão de UI: sem overshoot (Apple: damping ratio 1.0, response 0.3-0.4; em Motion `bounce: 0, duration: 0.4`). Bounce ~0.2 só quando o próprio gesto trouxe impulso (flick, arremesso).
- Referência Apple: mover/reposicionar damping 1.0 / response 0.4; rotação 0.8 / 0.4; drawer/sheet 0.8 / 0.3.

**Gestos (drag):** dispensar por velocidade, não só distância (`Math.abs(dist)/ms > 0.11` já fecha); `setPointerCapture` ao começar; ignorar segundo toque durante o drag; respeitar o ponto onde o dedo pegou; além do limite, resistência crescente (rubber-band) em vez de parede; passar a velocidade de soltura para o spring.

## 4. Receitas mínimas

**Botão pressionado**
```css
.btn { transition: transform 160ms var(--ease-out); }
.btn:active { transform: scale(0.97); }
```

**Dropdown / popover** (sai do gatilho)
```css
.pop { transform-origin: var(--transform-origin);
  transition: opacity 200ms var(--ease-out), transform 200ms var(--ease-out); }
.pop[data-starting-style], .pop[data-ending-style] { opacity: 0; transform: scale(0.95); }
```
Tooltip: igual com 125ms e `scale(0.97)`; `.tooltip[data-instant] { transition-duration: 0ms }` depois que o primeiro abriu.

**Modal** (centralizado, backdrop junto)
```css
.modal { transform-origin: center;
  transition: opacity 250ms var(--ease-out), transform 250ms var(--ease-out); }
.modal[data-starting-style], .modal[data-ending-style] { opacity: 0; transform: scale(0.96); }
.backdrop { transition: opacity 250ms var(--ease-out); }
```

**Drawer**
```css
.drawer { transition: transform 500ms var(--ease-drawer); }
.drawer[data-closed] { transform: translateY(100%); }
```

**Toast** (transition + `@starting-style`, nunca keyframe)
```css
.toast { transition: opacity 400ms ease, transform 400ms ease;
  @starting-style { opacity: 0; transform: translateY(100%); } }
```
400ms e `ease` são escolha de personalidade (Sonner, elegante). Sem suporte a `@starting-style`: `useEffect(() => setMounted(true), [])` + `data-mounted`.

**Tabs com indicador**: duplicar a lista de abas, estilizar a cópia como ativa e recortar só a aba ativa; texto e fundo trocam em sincronia.
```css
.tabs-ativa { clip-path: inset(0 60% 0 20%); /* calculado pela posição da aba */
  transition: clip-path 250ms var(--ease-in-out); }
```
Em Motion, `layoutId` também serve, mas Emil registra que shared layout animation perdeu frames no dashboard da Vercel durante carregamento; o clip-path em CSS roda fora da main thread.

**Lista entrando** (Motion, só em tela vista ocasionalmente)
```jsx
const pai = { show: { transition: { staggerChildren: 0.05 } } };
const item = { hidden: { opacity: 0, transform: "translateY(8px)" },
  show: { opacity: 1, transform: "translateY(0px)", transition: { duration: 0.3, ease: [0.23, 1, 0.32, 1] } } };
<motion.ul variants={pai} initial="hidden" animate="show">
  {itens.map(i => <motion.li key={i.id} variants={item} />)}
</motion.ul>
```

**Número / contador** (derivado do glossário: number ticker + tabular numbers; não há receita na fonte)
```jsx
const v = useMotionValue(0);
const txt = useTransform(v, n => Math.round(n).toLocaleString("pt-BR"));
useEffect(() => { const c = animate(v, alvo, { duration: 0.6, ease: [0.23, 1, 0.32, 1] }); return c.stop; }, [alvo]);
<motion.span style={{ fontVariantNumeric: "tabular-nums" }}>{txt}</motion.span>
```
Sempre `tabular-nums` para os dígitos não dançarem. Valor em `useMotionValue`, nunca em state.

**Skeleton / shimmer** (derivado: brilho é movimento constante = `linear`)
```css
.sk { background: linear-gradient(90deg, var(--sk) 0%, var(--sk-hi) 50%, var(--sk) 100%) 0 0 / 200% 100%;
  animation: sk 1.2s linear infinite; }
@keyframes sk { to { background-position: -200% 0; } }
@media (prefers-reduced-motion: reduce) { .sk { animation: none; } }
```

**Crossfade que não assenta**: `filter: blur(2px); opacity: 0.7` durante a troca (200ms `ease`) funde os dois estados num só. Blur sempre abaixo de 20px.

**Reveal com clip-path** (marketing): `clip-path: inset(0 0 100% 0)` → `inset(0 0 0 0)`, 600ms `--ease-in-out`, `useInView` com `{ once: true, margin: "-100px" }`.

## 5. Checklist de review (bloqueia se falhar)

1. Cada animação tem propósito nomeado e passa na tabela de frequência; nada anima em ação de teclado.
2. Sem `transition: all`: propriedades listadas.
3. Sem `ease-in` em UI; curvas são os tokens fortes, não `ease-out` nativo em animação deliberada.
4. Duração de UI ≤ 300ms ou com justificativa (modal/drawer até 500ms, marketing livre).
5. Sem `scale(0)` nem fade puro sem transform inicial.
6. Popover/dropdown/tooltip com `transform-origin` no gatilho; modal centralizado.
7. Elemento disparado rápido usa transition ou spring, não keyframe.
8. Só `transform`/`opacity` (clip-path é o quarto aceito; `height` só em accordion, curto).
9. Em Motion sob carga, string `transform` completa, não os atalhos `x`/`y`/`scale`.
10. Nenhuma variável CSS no pai dirigindo o transform dos filhos.
11. `prefers-reduced-motion` presente: mais suave, não zero (mantém opacity/cor, tira deslocamento).
12. Hover com movimento dentro de `@media (hover: hover) and (pointer: fine)`.
13. Press/hold/confirmação destrutiva com tempo assimétrico; saída pelo mesmo caminho da entrada.
14. Grupo entrando junto tem stagger de 30-80ms e não bloqueia clique.
15. Personalidade coerente: dashboard seco e rápido; produto lúdico pode ter bounce. Um componente saltitante num app sóbrio é defeito.
16. Conferência de sensação: rodar a 2-5x a duração ou no painel Animations do DevTools, quadro a quadro, gesto em aparelho real, e olhar de novo no dia seguinte.

Ordem de correção: apagar → reduzir → easing → origem/física → interrupção → GPU → tempo assimétrico → polimento (blur, stagger, `@starting-style`) → acessibilidade e coerência.

## 6. Vocabulário para briefar

- **Scale in / Pop in**: cresce ao aparecer / cresce com leve overshoot.
- **Reveal**: conteúdo descoberto por clip-path ou mask.
- **Origin-aware**: sai do gatilho, não do próprio centro.
- **Stagger**: itens em cascata com pequeno atraso. **Orchestration**: várias animações cronometradas como uma só.
- **Crossfade**: um some enquanto outro aparece no mesmo lugar. **Morph**: forma vira outra (Dynamic Island).
- **Shared element transition**: elemento viaja e se transforma (miniatura vira card). **Layout animation**: mudança de tamanho/posição anima em vez de pular.
- **Direction-aware**: avançar desliza para um lado, voltar para o outro.
- **Press feedback**, **Hold to confirm**, **Swipe to dismiss**, **Rubber-banding** (resistência e retorno além do limite), **Shake** (erro).
- **Spring**: stiffness (força), damping (quanto amortece; menos = mais quique), mass (peso), bounce, **momentum/velocity**, **interruptible**.
- **Number ticker** + **tabular numbers**; **Skeleton/shimmer**; **Text morph**; **Line drawing** (SVG se desenhando).

## 7. Performance específica

| Problema | Solução |
|---|---|
| Animação engasga | `transform`/`opacity`, nunca `width`/`top`/`margin`/`padding` |
| Motion `x`/`y` perde frame com a página ocupada | `animate={{ transform: "translateX(100px)" }}` |
| Propriedades aleatórias animando | listar propriedades, sem `all` |
| React re-renderiza a cada frame | escrever em `ref.current.style` ou motion value, não state |
| Elemento treme 1px ao começar | `will-change: transform`, só depois de ver o problema |
| Drag com muitos filhos lento | `el.style.transform` direto, não `setProperty('--x')` no pai |
| Blur pesado (Safari) | blur animado abaixo de 20px |

CSS e WAAPI rodam fora da main thread e continuam lisos enquanto a página carrega; `requestAnimationFrame` (Motion) perde frames. Movimento predeterminado em CSS, dinâmico/gesto em JS.

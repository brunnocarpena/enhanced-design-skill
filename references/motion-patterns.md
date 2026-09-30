# Padrões de motion (implementação)

Destilado da taste-skill, do frontend-design e do Emil Kowalski. Vale depois que o dial MOTION foi decidido em `aesthetic-direction.md`. Motion declarado no design.md tem que ser entregue; motion não declarado não entra.

Aprofundar só quando precisar:
- Micro-interação (botão, popover, modal, drawer, toast, tabs) e **revisão de qualquer animação** → [motion-craft.md](motion-craft.md) (tokens, receitas, checklist de 16 itens).
- Scroll narrativo, pin, scrub, timeline, SplitText, SVG → [gsap-scroll.md](gsap-scroll.md).
- MOTION 9-10, 3D, WebGL, sequência de frames, scroll-world → [cinematic-mode.md](cinematic-mode.md) (só com aceite registrado).

## Defaults
- Tokens: `--ease-out: cubic-bezier(0.23, 1, 0.32, 1)` (entrar/sair), `--ease-in-out: cubic-bezier(0.77, 0, 0.175, 1)` (mover na tela), `--ease-drawer: cubic-bezier(0.32, 0.72, 0, 1)`. Nunca `ease-in` em UI. Se o projeto já tem `--ease-*`, estender.
- Reveal: y 24 → 0 + opacity, 0.6s, `--ease-out`, stagger 30-80ms, dispara uma vez com 30% visível.
- Spring (Motion): `{ type: "spring", duration: 0.4, bounce: 0 }` em UI; bounce 0.1-0.3 só em gesto com impulso ou produto lúdico.
- Durações: pressionado 100-160ms (`scale(0.97)`), tooltip 125-200ms, dropdown 150-250ms, modal/drawer 200-500ms. UI fica abaixo de 300ms.
- Hover com movimento só em `@media (hover: hover) and (pointer: fine)`.
- Animar só `transform`, `opacity` (e `clip-path`). Nunca `transition: all`, nunca width/height/top/left. Nunca `scale(0)`: começar de 0.9-0.97.
- Ação disparada por teclado não anima. Elemento disparado rápido usa transition, não keyframe.
- `will-change` só no elemento que anima, e só depois de ver o problema.

## Motion (Framer Motion)
- `whileInView` + `viewport={{ once: true, amount: 0.3 }}` para reveal.
- Sob carga, string `transform` (`transform: "translateY(8px)"`) em vez dos atalhos `x`/`y`, que perdem frame.
- Valor contínuo (scroll, contador) em `useMotionValue`/`useScroll`, nunca em `useState`.
- `staggerChildren`: pai e filhos no mesmo client component.
- Tabs com indicador: preferir `clip-path` em CSS a `layoutId` (roda fora da main thread). `layout`/`layoutId` só em mudança de estado visível.
- Loop infinito (marquee, shimmer do CTA) isolado num componente folha, com `React.memo`.

## GSAP (resumo; detalhe em gsap-scroll.md)
- Todos os plugins são gratuitos. Versão exata via `supply-chain-guard`.
- Em React: `useGSAP` de `@gsap/react` (reverte no unmount; handler tardio com `contextSafe`).
- `gsap.quickTo` só para hover magnético ou tilt, nunca para cursor customizado.
- ScrollSmoother e Observer só com pedido explícito (flertam com scroll-jacking).
- Nunca misturar GSAP/Three com Motion na mesma árvore. Lib pesada via import dinâmico.

## Proibido
- Listener de `scroll` manual ou `window.scrollY` em state.
- Scroll-jacking (roubar a rolagem do usuário), inclusive no modo cinematográfico.
- Animar tudo ao entrar na página. Um momento orquestrado vale mais que dez fades.
- Cursor customizado.
- Dado que o usuário lê (gráfico, tabela, saldo) mexendo por estilo.

## prefers-reduced-motion
Obrigatório em toda animação: cortar deslocamento, parallax, pin e loops; manter opacity/cor curta. Marquee e loops param. No modo cinematográfico, a versão reduzida é estática de verdade (posters + texto). Testar com a preferência ligada.

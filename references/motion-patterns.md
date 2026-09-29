# Padrões de motion (implementação)

Destilado da taste-skill e do frontend-design. Vale depois que o dial MOTION foi decidido em `aesthetic-direction.md`. Motion declarado no design.md tem que ser entregue; motion não declarado não entra.

## Defaults
- Reveal: y 24 → 0 + opacity, 0.6s, `cubic-bezier(0.16, 1, 0.3, 1)`, stagger 60ms, dispara uma vez com 30% visível.
- Spring (Motion/Framer): stiffness 100, damping 20. Nada de bounce elástico em interface séria.
- Micro-interação: 150-300ms. Pressionado: `scale(0.98)`. Hover só em `@media (hover: hover)`.
- Animar só `transform` e `opacity`. Nunca `transition: all`, nunca width/height/top/left.
- `will-change` só no elemento que anima, e só enquanto anima.

## Motion (Framer Motion)
- `whileInView` + `viewport={{ once: true, amount: 0.3 }}` para reveal.
- Valor contínuo (cursor, scroll) em `useMotionValue`/`useScroll`, nunca em `useState` (re-render a cada frame).
- `staggerChildren`: pai e filhos no mesmo client component.
- `layout`/`layoutId` só em mudança de estado visível (tabs, reorder), não em tudo.
- Loop infinito (marquee, shimmer do CTA) isolado num componente folha, com `React.memo`, para não re-renderizar a árvore.

## GSAP + ScrollTrigger (só quando o brief pede scroll narrativo)
- Pin: `start: "top top"`, `pin: true`, `end: "+=" + distancia`, `invalidateOnRefresh: true`.
- Em React: `gsap.context()` no efeito e `ctx.revert()` no cleanup.
- Nunca misturar GSAP/Three com Motion na mesma árvore. Lib pesada via import dinâmico.

## Proibido
- Listener de `scroll` manual ou `window.scrollY` em state.
- Scroll-jacking (roubar a rolagem do usuário).
- Animar tudo ao entrar na página. Um momento orquestrado vale mais que dez fades.
- Cursor customizado.

## prefers-reduced-motion
Com MOTION > 3, obrigatório: cortar deslocamento e parallax, manter só opacity curta. Marquee e loops param. Testar com a preferência ligada.

# GSAP + ScrollTrigger (oficial GreenSock)

Ler quando o brief pede scroll narrativo, pin, scrub, timeline complexa, texto com SplitText ou SVG. Reveal simples continua em `motion-patterns.md`.

## Licença e instalação
- GSAP e TODOS os plugins (SplitText, MorphSVG, DrawSVG...) são gratuitos, inclusive comercial, desde a aquisição pela Webflow.
- Instalar só do pacote público `gsap` com versão exata (`npm i gsap@3.15.0 @gsap/react@2.1.2 --save-exact`; conferir com `npm view gsap version`). Plugins vêm dentro (`gsap/SplitText`).
- Nunca `.npmrc`, token ou registry `npm.greensock.com` (obsoleto).

## Setup
**HTML estático** (jsDelivr, versão fixa, `defer`, plugins depois do core):
```html
<script defer src="https://cdn.jsdelivr.net/npm/gsap@3.15.0/dist/gsap.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/npm/gsap@3.15.0/dist/ScrollTrigger.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/npm/gsap@3.15.0/dist/SplitText.min.js"></script>
<script defer src="/js/anim.js"></script> <!-- gsap.registerPlugin(ScrollTrigger, SplitText) no topo -->
```
Em produção, somar `integrity="sha384-..." crossorigin="anonymous"` (SRI).

**Next.js (App Router) / React**: componente client, registrar uma vez no nível do módulo (nunca dentro do componente), `useGSAP` no lugar de `useEffect`:
```tsx
"use client";
import { useRef } from "react";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { useGSAP } from "@gsap/react";
gsap.registerPlugin(useGSAP, ScrollTrigger); // useGSAP também é plugin

export function Secao() {
  const root = useRef<HTMLElement>(null);
  const { contextSafe } = useGSAP(() => {
    gsap.from(".card", { y: 40, autoAlpha: 0, stagger: 0.1,
      scrollTrigger: { trigger: ".grade", start: "top 80%" } });
  }, { scope: root });                       // seletores ficam presos ao componente
  const onClick = contextSafe(() => gsap.to(".card", { scale: 0.98 })); // handler criado depois
  return <section ref={root}>...</section>;
}
```
- `useGSAP` reverte tudo no unmount (opções `dependencies`, `scope`, `revertOnUpdate`); handler criado depois precisa de `contextSafe`.
- Sem `@gsap/react`: `const ctx = gsap.context(() => {...}, ref); return () => ctx.revert();` dentro do `useEffect`.
- Nada de GSAP no servidor (SSR). Componente pesado via `next/dynamic`.

## Core
- `to` (atual → vars), `from` (vars → atual; entrada), `fromTo` (explícito), `set` (duração 0). camelCase. Preferir aliases de transform: `x`, `y` (px), `xPercent`, `yPercent` (%; funcionam em SVG), `scale`, `rotation`, `rotationX/Y`, `skewX`, `transformOrigin`. Relativos: `x: "+=20"`.
- `autoAlpha` no lugar de `opacity`: em 0 aplica `visibility: hidden` e o elemento para de bloquear clique.
- Eases: `power1.out` (padrão), `power2.out`/`power3.out` para entrada, `power3.inOut` para transição entre estados, `expo.out` para reveal marcado, `none` para scrub e scroll horizontal. `back`/`elastic` só em UI lúdica. Token da casa `--ease-out` (`cubic-bezier(0.23,1,0.32,1)`) via `CustomEase.create("saida", ".23,1,.32,1")`.
- `gsap.defaults({ duration: 0.6, ease: "power2.out" })`. `stagger: 0.08` ou `{ each: 0.08, from: "center" }` / `{ amount: 0.4 }`.
- Vários `from()` na mesma propriedade do mesmo alvo: `immediateRender: false` nos seguintes. Sequência = timeline, nunca `delay`.
- Valor que muda muito (hover magnético, tilt): `gsap.quickTo`, que reaproveita um tween só. Nunca para cursor customizado (proibido na casa).
```js
const xTo = gsap.quickTo(el, "x", { duration: 0.4, ease: "power3" });
const yTo = gsap.quickTo(el, "y", { duration: 0.4, ease: "power3" });
area.addEventListener("pointermove", e => { const r = area.getBoundingClientRect();
  xTo((e.clientX - r.left - r.width / 2) * 0.2); yTo((e.clientY - r.top - r.height / 2) * 0.2); }); // magnético, só com (hover: hover) and (pointer: fine)
```

## Timeline
- `gsap.timeline({ defaults: { duration: 0.5, ease: "power2.out" } })`; duração da timeline = soma dos filhos.
- Position parameter (3º argumento): `1` absoluto; `"+=0.5"`/`"-=0.2"` relativo ao fim; `"<"` junto com o anterior; `">"` depois do anterior (padrão); `"<0.2"`; `"passo2+=0.3"` (label).
- Labels: `addLabel("passo2")`, `play("passo2")`; com scroll, `snap: "labels"`.
- ScrollTrigger só na timeline ou tween de nível mais alto, nunca em tween filho.

## ScrollTrigger
- `start`/`end`: `"<ponto do trigger> <ponto da viewport>"`. Ex.: `"top 80%"`, `"top top"`, `"bottom center"`, `"+=1500"` (px após o start), `"+=200%"` (altura do scroller), `"clamp(top bottom)"`. Padrão: start `"top bottom"` (`"top top"` com pin), end `"bottom top"`.
- `toggleActions: "play none none reverse"` (onEnter, onLeave, onEnterBack, onLeaveBack). Padrão `"play none none none"`. `once: true` para disparar uma vez.
- `scrub: true` = progresso colado ao scroll; `scrub: 1` = 1s para alcançar (mais suave e mais leve). Nunca `scrub` e `toggleActions` juntos (scrub vence).
- `pin: true` fixa o trigger durante o intervalo; `pinSpacing: true` (padrão) cria o espaçador para o layout não colapsar. `false` só se o layout já resolve.
- `snap: 0.25`, `[0, 0.5, 1]`, `"labels"` ou `{ snapTo: "labels", duration: 0.3, ease: "power1.inOut" }`.
- `markers: true` só em dev. `invalidateOnRefresh: true` quando o valor é função da tela.
- `refresh()` é automático no resize (200ms). Manual após fontes/imagens/conteúdo novo: `document.fonts.ready.then(() => ScrollTrigger.refresh())` e no `load`.
- Criar triggers de cima para baixo; fora de ordem (async), `refreshPriority` (primeiro na página = menor).
- Sem animação: `ScrollTrigger.create({ onUpdate: self => self.progress })`; menu ativo por seção com `toggleClass`.
- `ScrollTrigger.batch(".card", { start: "top 85%", onEnter: els => gsap.to(els, { autoAlpha: 1, y: 0, stagger: 0.1, overwrite: true }) })`: um trigger por item, callbacks agrupados; substitui IntersectionObserver. Não aceita `scrub`/`snap`/`toggleActions`.
- Responsivo e acessível com `gsap.matchMedia()` (reverte sozinho; não aninhar `gsap.context`):
```js
const mm = gsap.matchMedia();
mm.add({ desk: "(min-width: 800px)", reduz: "(prefers-reduced-motion: reduce)" }, (ctx) => {
  const { desk, reduz } = ctx.conditions;
  if (reduz) { gsap.set(".passo", { autoAlpha: 1 }); return; } // sem pin, sem deslocamento
  if (desk) { /* pin + scrub */ } else { /* reveal simples, sem pin */ }
});
```

**Erros comuns**: animar o elemento pinado (animar filhos ou pinar o wrapper); ScrollTrigger em tween filho; triggers fora de ordem (espaçador desalinha o resto); não matar no unmount/troca de rota (`useGSAP`, `ctx.revert()`, `ScrollTrigger.getById(id)?.kill()`); esquecer `registerPlugin`/`refresh()`; horizontal com ease diferente de `"none"`.

## Plugins (todos em `gsap/<Nome>`, registrar antes de usar)
- **SplitText**: quebra em `chars`, `words`, `lines`. Dividir só o que anima. `mask: "lines"` cria wrapper com `overflow: clip` (reveal por máscara). `autoSplit: true` re-divide quando fontes carregam ou a largura muda; nesse caso criar a animação dentro de `onSplit` e retorná-la. `aria: "auto"` (padrão) preserva leitor de tela. Sem `text-wrap: balance` no elemento. Não serve para `<text>` SVG.
- **Flip**: `const s = Flip.getState(".item")` → muda DOM/classe → `Flip.from(s, { duration: 0.5, ease: "power2.inOut", absolute: true })`. Filtro de grade, card que expande, reorder.
- **ScrollSmoother**: rolagem suavizada; exige ScrollTrigger e a estrutura `#smooth-wrapper > #smooth-content` (fixos ficam fora). Ver conflito com "scroll-jacking" em `motion-patterns.md`: só com pedido explícito.
- **DrawSVG**: `gsap.from("#traco", { drawSVG: 0, duration: 1.2 })`; o valor é o trecho visível (`"0% 100%"`, `"20% 80%"`). Precisa de `stroke` e `stroke-width`. Só stroke, não fill.
- **MorphSVG**: `gsap.to("#a", { morphSVG: "#b", ease: "power2.inOut" })`. Formas primitivas: `MorphSVGPlugin.convertToPath("circle, rect")`. Morph torcido: `shapeIndex: "log"` e fixar o valor.
- **MotionPath**: `motionPath: { path: "#rota", align: "#rota", alignOrigin: [0.5, 0.5], autoRotate: true }`.
- **Observer**: normaliza wheel/touch/pointer (`onUp`, `onDown`, `tolerance: 10`). Para swipe; nunca trocar seção a cada rodinha (scroll-jacking).
- **ScrollToPlugin** (`scrollTo: { y: "#secao", offsetY: 80 }`) para âncora animada. **GSDevTools** só em dev.

## Performance
- Animar só `transform` e `opacity` (`x`, `y`, `scale`, `rotation`, `autoAlpha`). Evitar `width`, `height`, `top`, `left`, `margin`, `padding`.
- `will-change: transform` só no elemento que de fato anima; nada de `force3D` em tudo.
- Não intercalar leitura e escrita de layout. `stagger` em vez de tweens com delay; nunca criar timeline por frame.
- Lista longa: animar só o visível (batch). Pin só no necessário; `scrub: 1` alivia; testar em celular fraco. Matar o que saiu da rota.

## Receitas
**Hero com texto por linhas**
```js
SplitText.create(".hero h1", { type: "lines", mask: "lines", autoSplit: true,
  onSplit: s => gsap.from(s.lines, { yPercent: 110, duration: 0.9, ease: "expo.out", stagger: 0.08 }) });
```
Não esconder o H1 no CSS (derruba o LCP); animação curta, sem delay.

**Seção pinada com passos**
```js
const tl = gsap.timeline({ scrollTrigger: { trigger: ".passos", start: "top top",
  end: "+=" + innerHeight * 3, pin: true, scrub: 1, snap: "labels", invalidateOnRefresh: true } });
tl.addLabel("p1").from(".p1", { autoAlpha: 0, y: 40 })
  .addLabel("p2").to(".p1", { autoAlpha: 0 }).from(".p2", { autoAlpha: 0, y: 40 }, "<")
  .addLabel("p3").to(".p2", { autoAlpha: 0 }).from(".p3", { autoAlpha: 0, y: 40 }, "<");
```

**Scroll horizontal de cards** (pina o wrapper, anima a trilha filha)
```js
const trilha = document.querySelector(".trilha");
const dist = () => trilha.scrollWidth - innerWidth;
const h = gsap.to(trilha, { x: () => -dist(), ease: "none",
  scrollTrigger: { trigger: ".horizontal", pin: true, scrub: 1, end: () => "+=" + dist(), invalidateOnRefresh: true } });
gsap.from(".card-destaque", { y: 60, scrollTrigger: { containerAnimation: h, trigger: ".card-destaque", start: "left center" } });
```
Triggers com `containerAnimation` não aceitam pin nem snap. No mobile, scroll nativo com `scroll-snap` (matchMedia).

**Parallax de camadas**
```js
gsap.utils.toArray("[data-vel]").forEach(el => gsap.to(el, { yPercent: -20 * el.dataset.vel, ease: "none",
  scrollTrigger: { trigger: el.closest("section"), start: "top bottom", end: "bottom top", scrub: true } }));
```

**Contador ao entrar**
```js
const n = { v: 0 };
gsap.to(n, { v: +el.dataset.alvo, duration: 1.6, ease: "power2.out",
  onUpdate: () => el.textContent = Math.round(n.v).toLocaleString("pt-BR"),
  scrollTrigger: { trigger: el, start: "top 85%", once: true } });
```
Número real no HTML (SEO, leitor de tela, reduced motion); a animação parte de 0 só no cliente.

**Sequência de imagens em canvas por scrub**
```js
const frames = [...Array(120)].map((_, i) => Object.assign(new Image(), { src: `/seq/${String(i+1).padStart(3,"0")}.webp` }));
const f = { i: 0 }, ctx2d = canvas.getContext("2d");
const draw = () => ctx2d.drawImage(frames[f.i], 0, 0, canvas.width, canvas.height);
frames[0].onload = draw;
gsap.to(f, { i: frames.length - 1, snap: "i", ease: "none", onUpdate: draw,
  scrollTrigger: { trigger: ".seq", start: "top top", end: "+=300%", pin: true, scrub: 0.5 } });
```
WebP, 60-150 quadros, pré-carregar perto da seção; imagem estática no reduced motion.

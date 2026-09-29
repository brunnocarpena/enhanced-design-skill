# Presets de estilo (ponto de partida, não receita)

Destilado das variantes da taste-skill (soft, minimalist, brutalist, stitch). Usar **só quando o projeto não tem identidade definida** (sem Claude Design, sem paleta de marca). Escolher um na Fase 1, justificar pelo brief e ajustar no Passe 2 de `aesthetic-direction.md`. Nunca misturar dois. **Nenhum preset** é resposta válida: se o Passe 2 trocou dois ou mais eixos do preset (fundo, tipografia, acento), declarar "sem preset" no design.md e ficar só com os tokens transversais.

Sobre qualquer preset valem as regras da casa: CTA primário CAIXA ALTA + shimmer (do preset herda só cor, raio e ícone), grade auto-fit, sem max-width de página, mobile-first, zero número fake, fonte mínima 12px em rótulo e 16px em input.

## P1. Soft / Ethereal Glass (escuro atmosférico)
- **Para**: SaaS premium, IA, lançamento com clima noturno.
- **Tokens**: fundo #050505; orbs de mesh radial desfocados atrás do conteúdo (nunca sobre texto); hairline rgba(255,255,255,.10); easing cubic-bezier(0.32,0.72,0,1) em 700ms.
- **Assinatura**: double-bezel em card de destaque (casca externa white/5 + ring-1 + p-1.5, raio 2rem; núcleo com raio `calc(2rem - 0.375rem)` e `inset 0 1px 1px rgba(255,255,255,.15)`). Raio concêntrico: interno = externo - padding.
- **Regra**: backdrop-blur só em elemento fixed/sticky (nav, sheet), nunca em card que rola.
- **Estraga**: glass empilhado sem motivo, glow em borda, blur em muitos elementos (FPS no mobile).

## P2. Soft / Editorial Luxury (claro quente)
- **Para**: mentoria high-ticket, marca pessoal, bem-estar premium, página de vendas "de revista".
- **Tokens**: fundo #FDFBF7; acento sálvia ou espresso; serif variável nos títulos só se a marca comporta; sans de apoio Geist ou Plus Jakarta Sans; grain 0.03.
- **Assinatura**: Editorial Split no desktop (tipografia grande numa metade, imagem na outra), empilhado no mobile; nav em pílula flutuante; eyebrow em pill no máximo 1 a cada 3 seções.
- **Cuidado**: é exatamente o default de calibração nº 1 (creme + serif). Só usar com justificativa escrita do brief e trocar pelo menos um dos três (fundo, serif, acento).
- **Estraga**: "luxo" que é só bege + serif sem hierarquia; tudo animando ao entrar.

## P3. Soft Structuralism (claro neutro)
- **Para**: produto confiável, fintech leve, área do cliente com cara de marketing.
- **Tokens**: fundo prata/branco; grotesk forte nos títulos (Geist, Clash Display); sombra ambiente difusa e tingida pelo fundo.
- **Assinatura**: bento assimétrico via auto-fit + `grid-column: span 2` no item de destaque (nunca colunas travadas por breakpoint); hover magnético no ícone do botão (`translate-x-1 -translate-y-px`), `active:scale-[0.98]`.
- **Estraga**: borda cinza 1px genérica em todo card; transição linear.

## P4. Minimalist utilitário (Notion/Linear)
- **Para**: ferramenta interna, docs, central de ajuda, painel admin, formulários.
- **Tokens**: canvas #FFFFFF/#F7F6F3/#FBFBFA; superfície #F9F9F8; borda #EAEAEA; texto #2F3437 (line-height 1.6), secundário #787774. Pastéis de estado (fundo/texto): vermelho #FDEBEC/#9F2F2D, azul #E1F3FE/#1F6C9F, verde #EDF3EC/#346538, amarelo #FBF3DB/#956400.
- **Assinatura**: sombra < 0.05 de opacidade; card raio 8-12px, padding 24-40px; FAQ só com border-bottom e ícone +/-; `<kbd>` estilizado; entrada translateY(12px) + opacity, 600ms, stagger `calc(var(--index) * 80ms)`.
- **Estraga**: gradiente, neon, glass, card rounded-full, foto saturada.

## P5. Brutalist Swiss Print (claro)
- **Para**: evento, lançamento com atitude, agência, produto técnico com voz forte. **Nunca** para nicho sensível (terapia, saúde, mentoria feminina).
- **Tokens**: fundo #F4F4F0/#EAE8E3; tinta #111; UM acento vermelho #E61919. Macro: Archivo Black/Monument Extended, `clamp(4rem,10vw,15rem)`, tracking -0.04em, line-height 0.9. Micro: JetBrains/IBM Plex Mono.
- **Assinatura**: raio 0; grade blueprint (`display:grid; gap:1px; background: <cor da linha>`); densidade bimodal; `<data>`, `<samp>`, `<dl>` para dado técnico.
- **Casa**: títulos não ficam todos em caixa alta; micro-texto nunca abaixo de 12px.
- **Estraga**: sombra suave, gradiente, translucidez, segundo acento.

## P6. Brutalist Telemetry (escuro CRT)
- **Para**: painel "ao vivo", operações, página de dados.
- **Tokens**: fundo #0A0A0A/#121212; texto #EAEAEA; acento vermelho; verde #4AF626 em no máximo UM elemento (e só se for estado ao vivo real).
- **Assinatura**: scanlines `repeating-linear-gradient(0deg, transparent 0 2px, rgba(0,0,0,.1) 2px 4px)` em pseudo-elemento fixo; números em `tabular-nums`.
- **Estraga**: efeito CRT atrapalhando leitura; verde espalhado.

## P7. Calm Premium SaaS (default limpo)
- **Para**: app ou SaaS quando o brief não pede nada.
- **Tokens**: canvas #F9FAFB; superfície #FFF; texto #18181B; secundário #71717A; borda rgba(226,232,240,.5); sombra `0 20px 40px -15px rgba(0,0,0,.05)`; display `clamp(2.25rem,5vw,3.75rem)`, tracking -0.025em; body line-height 1.65. Um acento: Emerald #10B981, Blue #3B82F6, Rose #E11D48 ou Amber #F59E0B.
- **Regras**: em dashboard, nada de serif; raio proporcional ao tamanho (2.5rem em card, nunca em botão pequeno); ritmo de seção `clamp(3rem,8vw,6rem)`; padding lateral 1rem/2rem/4rem.

## Tokens transversais (qualquer preset)
- Nada de #000/#fff puro como superfície. Uma família de cinza (quente ou fria). Acento < 80% de saturação.
- Sombra tingida, uma única direção de luz.
- Pesos 500/600 para hierarquia. Tracking negativo no display, positivo em rótulo.
- Ajuste óptico: padding inferior 1-2px maior; ícone em botão com nudge de 1px.
- Botões alinhados no rodapé dos cards (`flex-direction: column` + `margin-top: auto`); listas começando no mesmo Y.
- Ícones: manter a lib do projeto (shadcn = Lucide), stroke único. Projeto novo sem lib: Phosphor.

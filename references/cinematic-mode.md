# Modo cinematográfico (3D, WebGL e scrollytelling)

Para o cliente que quer um site "de cinema" (voo pela cena, objeto 3D girando com o scroll, mundo em camadas) e **aceita trocar performance por impacto**. Complementa [motion-patterns.md](motion-patterns.md): lá estão os defaults de motion, e o GSAP está em [gsap-scroll.md](gsap-scroll.md); aqui entra só o que é específico de 3D, WebGL e scrub de vídeo/frames.

## 1. Quando ativar

Só com UMA das duas condições:
- **Pedido explícito** do cliente/usuário ("quero 3D", "site tipo Apple", "voar pela cena").
- **Dial MOTION 9-10** + aceite registrado no `design.md`: `Performance: segundo plano (aceite de <quem>, <data>)`. Sem essa linha, o gate da Fase 5 continua valendo integralmente.

**Nunca** em página de captura ou de vendas com tráfego pago mobile, a menos que o usuário peça com todas as letras. Nesses casos, oferecer o motor mais leve da matriz (CSS nativo ou 2.5D) e dizer o custo do resto.

Mesmo ativo, regras da casa e QA da Fase 4 continuam; só a performance passa de "bloquear" para "medir e declarar".

## 2. Matriz de escolha do motor

| Motor | Resultado visual | Custo de performance | Custo de produção | Quando usar |
|---|---|---|---|---|
| **Sequência de frames em canvas** (vídeo → frames) | Parece 3D fotorrealista; é um vídeo "rebobinado" pelo scroll | Alto em rede (8-20 MB por seção), baixo em CPU | Vídeo pronto: horas. Vídeo IA: créditos (~54 por clipe 1080p no Higgsfield) + 3-8 min por render | Produto girando, explosão de peças, reveal. Um ou dois momentos fortes na página |
| **Scroll-world** (vídeo pré-renderizado com scrub por seções) | Câmera voa de cena em cena sem corte, mundo contínuo | Alto em rede (vários MP4 de ~8 MB), médio em decodificação no celular | O mais caro: N imagens + até 2N-1 vídeos IA, com re-rolls; 1-2 dias | Contar a cadeia de valor de um negócio como uma viagem (fazenda → cozinha → loja) |
| **Three.js / React Three Fiber** | 3D real, interativo (girar com o dedo, trocar cor, hover) | Alto em CPU/GPU e JS (three.js sozinho ~150 KB gz); bateria | Precisa do modelo GLB (cliente, modelagem 3D ou marketplace); dias de dev | Configurador de produto, objeto que o usuário manipula, cena que reage ao cursor |
| **Spline** | 3D estilizado, montado no editor visual | Alto: runtime pesado + arquivo da cena; pouco controle fino | Baixo em código, médio em design (alguém monta no editor) | Hero 3D decorativo feito por designer, protótipo rápido, cliente que quer editar sozinho |
| **GSAP ScrollTrigger 2.5D** | Profundidade por camadas PNG/WebP (parallax, push-in, portal abrindo) | Médio: só transform/opacity; imagens são o peso | Camadas recortadas e alinhadas (IA ou foto); 1-2 dias | Editorial/turismo/marca pessoal; o "3D" mais barato que ainda impressiona |
| **CSS scroll-driven animations nativo** | Parallax, reveal e progresso ligados ao scroll, sem JS | Mínimo (roda fora da main thread) | Horas | Toque cinematográfico em página que ainda precisa converter; base de qualquer outro motor |

Regra de desempate: **o motor mais leve que entrega o efeito pedido**. Vídeo (frames ou scroll-world) quando o visual é fotorreal e não precisa de interação; Three.js quando precisa de interação; 2.5D quando o orçamento de créditos é zero.

## 3. Pipeline: sequência de frames em canvas

Não é Three.js: um clipe curto vira quadros numerados, e o quadro desenhado no `<canvas>` sai do progresso do scroll. O "3D" vem inteiro do vídeo.

**3.1 Obter o vídeo** (movimento contínuo, sem corte: corte fica feio rebobinado)
- Vídeo do cliente ou render próprio (Blender, Remotion): custo zero de créditos.
- IA (Higgsfield): **só com ok explícito do usuário e custo estimado antes** (ex.: "2 clipes 1080p ≈ 108 créditos + re-rolls"). Consultar o custo pela própria ferramenta antes de gerar. Falha por moderação costuma ser reembolsada; avisar cada re-roll, nunca dizer que renderizou se não renderizou.
- Movimentos que funcionam: turntable 360°, dolly lento para dentro, peças se separando em câmera lenta.

**3.2 Extrair os frames** (ffmpeg via `brew install ffmpeg`; nunca baixar binário estático de site de terceiro)
```bash
# ~150 quadros espaçados por igual num clipe de 6 s (150/6 = 25 fps)
ffmpeg -i hero.mp4 -vf "fps=25,scale=1600:-2" -q:v 3 frames/hero/f_%04d.jpg
# versão mobile: metade dos quadros, 900px
ffmpeg -i hero.mp4 -vf "fps=12.5,scale=900:-2" -q:v 4 frames/hero-m/f_%04d.jpg
```

**3.3 Comprimir**: converter para WebP (`cwebp -q 78`) ou AVIF; 1600px desktop, ~900px mobile. Alvo: **≤ 60 KB por quadro, ≤ 12 MB por seção desktop, ≤ 4 MB mobile**. Mais de ~180 quadros quase nunca melhora a sensação e sempre piora o carregamento.

**3.4 Carregar em ondas**: quadro 0 primeiro (vira o poster), depois um a cada 8, depois o resto. O scrub usa o quadro carregado mais próximo enquanto a sequência completa chega. Só começar a baixar quando a seção estiver perto da viewport (IntersectionObserver com `rootMargin` de 1 tela).

**3.5 Desenhar**: canvas com `width = clientWidth * min(devicePixelRatio, 2)`, desenho em "cover" (recorte centralizado) e redesenho **só quando o índice muda**, dentro de `requestAnimationFrame`.

**3.6 Ligar ao scroll**: seção externa com 400-600vh, palco interno `position: sticky; top: 0; height: 100svh`. Progresso = distância rolada dentro da seção ÷ (altura da seção - altura da tela), preso entre 0 e 1. Índice = `round(p * (total - 1))`. Com GSAP já no projeto, `ScrollTrigger` com `scrub: true` faz isso; sem GSAP, o cálculo acima num rAF basta (Lenis é opcional, não obrigatório).

**3.7 Fallback**: o quadro 0 é um `<img>` real no HTML (poster), com `width/height`, e só é escondido quando o canvas pinta. Sem JS, com reduced-motion ou com rede lenta (`navigator.connection.saveData`), o poster fica e a sequência nem baixa.

No Next: componente client via `dynamic(..., { ssr: false })`, poster renderizado no servidor.

## 4. Scroll-world (voo contínuo entre cenas)

Scroll controla o `currentTime` de vídeos encadeados: a câmera entra numa cena, sai e entra na próxima sem corte.

**Estrutura**: 5-7 seções, cada uma com rótulo, sobretítulo, título, uma frase e até 3 tags; a última é o produto + CTA. Todas as imagens de cena usam **o mesmo preâmbulo de estilo, palavra por palavra** (é isso que faz parecer um mundo só). Compor tudo centralizado: no celular em retrato o 16:9 perde as laterais.

**Costura (a parte que decide se fica bom)**
- A emenda tem que ser **o mesmo pixel dos dois lados**. Nunca usar a imagem original da cena como início/fim de um clipe de ligação: usar o **quadro realmente renderizado** do clipe vizinho (`ffmpeg -sseof -0.15 -i leg.mp4 -frames:v 1 last.png` para o último, `-ss 0` para o primeiro).
- **Arquitetura A (recomendada, realista)**: uma câmera que só avança. Cada trecho começa do último quadro real do anterior, sem imagem final forçada. Sequencial, não paraleliza. Pequeno crossfade (~8% do trecho) de seguro.
- **Arquitetura B (só miniatura/diorama)**: mergulho na cena + clipe de ligação que sobe e voa até a próxima. A câmera inverte o sentido em cada emenda; em cena realista isso parece rebobinar.
- Um único modelo de vídeo para a cadeia toda (trocar de modelo muda grão e cor e a emenda "pula"). Fazer uma prévia no modelo barato antes de gastar no final.

**Prompts de câmera**: "um único movimento contínuo, sem cortes"; no meio do trecho vale órbita, grua subindo ou travelling lateral (escolher pelo tema: produto = meia órbita, imóvel = steadicam por portas, indústria = travelling baixo); **o último segundo sempre volta a um avanço lento e constante**, e o trecho seguinte começa continuando esse avanço. Sem texto, sem letreiro.

**Encode**: `-an -c:v libx264 -crf 20 -g 8 -movflags +faststart` na resolução nativa; versão mobile 720p com `-g 4` (mais keyframes = seek mais barato no celular).

**O que o engine precisa fazer** (escrever o nosso ou adaptar, sem copiar o de terceiros): carregar cada clipe como `Blob` (seek funciona mesmo em host sem byte-range), pré-carregar só os clipes vizinhos, suavizar o `currentTime` no rAF, nunca pedir novo seek enquanto o anterior não terminou, manter a imagem da cena como poster até o vídeo pintar, "acordar" cada vídeo no primeiro toque no iOS (`muted playsinline`, play/pause), ignorar resize só de altura no mobile (barra de URL), e com reduced-motion mostrar só as imagens.

**Custo** para 6 cenas: 6 imagens + até 11 vídeos + ~1 re-roll por trecho expressivo. Interiores (quarto, piscina, spa) disparam falso positivo de moderação: prever re-rolls no orçamento.

## 5. Three.js essencial

**Caminho preferido no Next: React Three Fiber + drei**, carregado com `dynamic(..., { ssr: false })` e montado só quando o bloco entra na viewport. Three puro só em HTML estático.

- **Setup**: `WebGLRenderer({ antialias: true, powerPreference: 'high-performance' })`, `setPixelRatio(Math.min(devicePixelRatio, 2))` (1.5 no mobile), `outputColorSpace = SRGBColorSpace`, tone mapping ACES. Câmera perspectiva ~35-45° de FOV. Resize por `ResizeObserver` no container, não no `window`. No R3F: `<Canvas dpr={[1, 2]}>`.
- **Luz e material**: PBR (`MeshStandardMaterial`/`MeshPhysicalMaterial`) só fica bonito com **environment map**: HDR pequeno (1K) via PMREM, ou `<Environment>` do drei com arquivo local. Uma direcional para sombra + o environment; sombra real só onde se vê, senão contact shadow fake.
- **Modelo**: GLB com **Draco** (geometria) e **KTX2** (texturas), otimizado com gltf-transform. Alvo: ≤ 2-3 MB. Decoders Draco/Basis **servidos do próprio domínio** (copiar de `node_modules/three/examples/jsm/libs`), não de CDN externo. No R3F: `useGLTF` + `useGLTF.preload`.
- **Animação**: tudo no loop (`setAnimationLoop` / `useFrame`), com `delta` do clock; animações do GLB por `AnimationMixer`. Scroll → ref, nunca state. Pausar o loop quando o canvas sai da tela (`frameloop="demand"` no R3F + `invalidate()`).
- **Interação**: `Raycaster` com coordenadas normalizadas do container (no R3F, `onPointerOver/onClick` direto no mesh). `OrbitControls` com zoom desligado e ângulo limitado, senão sequestra o scroll da página.
- **Pós-processamento com moderação**: bloom sutil + SMAA/FXAA no máximo (cada passe re-renderiza a tela). DOF/SSAO só se o design.md pedir, nunca no mobile.
- **Dispose**: ao desmontar, `dispose()` de geometria, material, texturas e renderer. O R3F cuida do que ele criou; o criado à mão é nosso. Vazamento de GPU em SPA derruba a aba no celular.

## 6. Spline

Não há skill oficial; o abaixo é o comportamento conhecido, **conferir versão e API na hora de instalar** (passa pelo `supply-chain-guard`, versão exata).
- Pacotes: `@splinetool/runtime` (vanilla, `new Application(canvas).load(url)`) e `@splinetool/react-spline` (componente `<Spline scene="..."/>`; no Next há entrada específica para Next, conferir na doc).
- Export: no editor, "Export → Code" gera a URL `.splinecode`. Preferir **baixar o arquivo e servir do próprio domínio** a depender do CDN da Spline em produção.
- Eventos: `onLoad` (guardar a instância `app`), `app.findObjectByName()` para mexer em objetos, `onSplineMouseDown`/`emitEvent` para disparar estados criados no editor.
- Peso: runtime + cena passam fácil de 1-2 MB e rodam física/render em JS; sempre lazy (montar ao entrar na viewport), poster estático no lugar até o `onLoad`, e cena simplificada (menos objetos, sem sombras em tempo real) para mobile, ou só o poster.
- Remover a marca d'água depende do plano da conta Spline: checar antes de prometer.

## 7. Brief 2.5D (o que pedir ao cliente)

Antes de qualquer motor, fechar este brief (vale para todos, obrigatório no 2.5D):
- **Mensagem principal** e a **reação desejada** do visitante; CTA final.
- **Direção**: estilo, clima, luz/hora do dia, caráter de câmera e lente, paleta, fontes de título e de interface, **o que evitar**.
- **Batidas da narrativa** (5 costuma bastar): hero → primeira narrativa → revelação do mundo → segunda narrativa → catálogo/CTA final interativo. Para cada uma: título, texto, o que se move e como entra/sai.
- **Camadas** (2.5D): fundo/céu opaco, paisagem distante, meio, objeto herói recortado, oclusores de primeiro plano esquerdo/direito, moldura opcional. Todas com a mesma câmera, luz e proporção, 10-20% de sobra nas bordas, alpha limpo sem halo, **sem texto na imagem**.
- **Mobile** (recorte do sujeito em retrato, dispositivos mínimos) e **orçamento** (peso inicial, peso total, créditos de IA).
- **Restrições** (dependências, componentes intocáveis) e **critérios de aceite** mensuráveis.

Regras do motor 2.5D: palco sticky, progresso local da seção (0-1) num objeto de configuração legível, fundo move menos que o meio que move menos que o primeiro plano, parallax de ponteiro ≤ 24px e desligado em toque, blur só na transição (com fallback de opacidade), tudo reversível ao rolar para cima.

## 8. Guarda-corpos (valem mesmo no modo cinematográfico)

1. **LCP = poster**: primeiro quadro / imagem da cena como `<img>` real com `fetchpriority="high"` e dimensões. O motor pinta por cima depois.
2. **Motor fora da primeira dobra é lazy**; na primeira dobra, carregar depois do poster e do texto.
3. **prefers-reduced-motion**: versão estática de verdade (posters + texto em fluxo normal), não "a mesma coisa mais devagar".
4. **Sem WebGL** (checar `canvas.getContext('webgl2')`) ou falha de carregamento: cai no poster, nunca tela preta.
5. **Mobile mais leve**: metade dos quadros, 720p no vídeo, `dpr` ≤ 1.5, sem pós-processamento; `saveData` = só poster.
6. **Orçamento declarado no design.md** (ex.: "hero 10 MB desktop / 3 MB mobile, JS do motor 250 KB gz") e medido no QA. Estourou → reportar no resumo, com número.
7. **Cleanup**: remover listeners, cancelar rAF, `dispose` de GPU, `ctx.revert()` do GSAP, revogar object URLs de blob.
8. **SEO e a11y**: todo título, texto e CTA no DOM como HTML semântico, nunca só dentro do canvas ou do vídeo; canvas decorativo com `aria-hidden`; sem scroll-jacking; teclado alcança tudo.
9. **Dependência nova** (three, R3F, drei, GSAP, Lenis, Spline) → `supply-chain-guard`, versão exata, nada de `<script>` de unpkg sem SRI.
10. **Geração paga** só com ok e estimativa de créditos antes; registrar o gasto real no resumo.

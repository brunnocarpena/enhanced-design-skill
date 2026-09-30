# Corrigir sem quebrar (modos refine e audit)

Destilado do termos-ui-ux (Djuniorss, MIT) e filtrado pelas regras da casa. Vale quando o pedido é **consertar** algo que já existe: um print com defeito, "tá cortado", "tá por cima", "quebrou no celular". Serve para mim ao corrigir e para escrever o pedido quando outra IA ou pessoa vai executar.

## 1. Traduzir o sintoma

O cliente descreve o que vê; a correção depende do nome certo. Um sintoma leva a um termo principal e, no máximo, dois de apoio.

| O cliente diz | Termo | Correção na origem |
|---|---|---|
| "sobra espaço vazio dos lados", "ficou espremido no meio" | Full-width fluid | Tirar `max-width` do container; respiro vem do padding (regra da casa) |
| "os cards não se ajeitam", "3 por linha num monitor grande" | Grade auto-fit | `repeat(auto-fit, minmax(min(100%, var(--grade-min)), 1fr))` |
| "texto grande empurra a coluna pra fora" | `min-width: 0` | No item flex/grid e `minmax(0, 1fr)` na coluna |
| "nome comprido empurra o preço", "texto cortado com ..." | Wrapping | `overflow-wrap: anywhere`; o card cresce. Nunca truncate/line-clamp (casa) |
| "um card em cima do outro", "texto por cima de texto" | Normal flow + content-driven height | Tirar altura fixa e `absolute` estrutural; `min-height` no lugar de `height` |
| "no monitor grande virou uma coluna só" | Preserve composition | Voltar a composição; corrigir só a largura |
| "rola pro lado no celular", "a tela treme" | No horizontal scrolling | Achar o elemento mais largo que a viewport; nunca `overflow-x: hidden` global |
| "aparecem duas barras de rolagem" | Um dono de scroll por eixo | Só um nível com `overflow-y: auto` |
| "no fim da lista a página de trás rola" | Scroll containment | `overscroll-behavior: contain` na área interna |
| "o cabeçalho fixo cobre o conteúdo", "âncora para embaixo do menu" | Sticky x fixed | `sticky` no fluxo; `scroll-margin-top` nas âncoras |
| "o menu abre atrás", "o dropdown aparece cortado" | Stacking context x overflow | Ver seção 4 |
| "o botão fica atrás da barrinha do iPhone" | Safe area | `env(safe-area-inset-bottom)` + `viewport-fit=cover` |
| "ao tocar no campo o iPhone dá zoom" | iOS input zoom | Fonte de input ≥ 16px; nunca bloquear zoom |
| "o teclado cobre o campo / o botão de enviar" | Virtual keyboard overlap | `dvh`, sem barra `fixed` no rodapé em tela de digitação, `scrollIntoView` no foco |
| "o botão responde atrasado no celular" | Tap delay | `touch-action: manipulation` nos controles |
| "a foto está esticada / achatada" | object-fit | `cover` em foto, `contain` em logo/produto/print; `height: auto` com width/height no `<img>` |
| "o corte tirou a cabeça da pessoa" | object-position | Ponto de interesse por imagem; conferir no mobile e no desktop |
| "fotos da mesma linha com alturas diferentes" | aspect-ratio | Proporção fixa na grade, largura fluida |
| "o título some na parte clara da foto" | Scrim | Degradê só na região do texto; contraste AA |
| "a página pula enquanto carrega" | CLS | Reservar espaço (width/height, aspect-ratio, altura de embed) |
| "a lista de depoimentos fica enorme no celular" | Carrossel scroll-snap | Ver seção 5 |
| "o vídeo do hero não toca no iPhone" | Background video | `muted playsinline autoplay loop` + poster + fallback em reduced-motion |

Sintoma que não está aqui: descrever o que se vê, nomear a propriedade CSS responsável e só então mexer.

## 2. Auditoria de print (modo audit ou print do cliente)

1. Listar só o que **aparece** no print. Hover, código e comportamento no celular que não dá para ver viram "conferir", não afirmação.
2. Ordenar por impacto:
   - **Quebrado**: sobreposição, texto cortado, rolagem lateral, conteúdo inacessível.
   - **Atrapalha a tarefa**: ação principal escondida ou sem destaque, falta de feedback, pessoa sem saber onde está.
   - **Acabamento**: espaçamento irregular, hierarquia fraca, cor/fonte inconsistente.
3. Uma linha por item: *o que se vê → termo → correção*. Teto de ~7 itens.
4. Corrigir **um defeito de layout por vez**, do mais grave para o menos, validando entre eles. Dez correções de layout num passo só é o que mais quebra a tela. Acabamento independente (cor, texto, ícone) pode ir junto.

## 3. Os três blocos de toda correção de layout

Obrigatórios em correção de layout, grid, scroll ou responsividade, seja eu executando ou escrevendo o pedido para outro executor.

**Preserve** - o que já está certo e não pode mudar. Se ninguém disse, usar o padrão seguro: a composição atual, cores, fontes e o comportamento das outras telas. Em componente compartilhado, corrigir primeiro só na tela-alvo e depois avaliar subir para o layout comum.

**Não use** (atalhos que parecem resolver no print e quebram em outra resolução):
- `transform: scale()` ou `zoom` para caber;
- fonte menor para caber;
- `position: absolute` para montar estrutura;
- margem negativa;
- `overflow: hidden` para esconder o defeito (permitido no contêiner externo quando há área interna que rola, e para recortar mídia com canto arredondado);
- altura fixa menor que o conteúdo;
- `z-index: 9999` no chute;
- e, pela casa: `max-width` de página, truncate/line-clamp, colunas travadas por breakpoint.

**Validação** - compilar não é concluir. Renderizar e ver prints; informar a causa raiz e os arquivos alterados. As resoluções mudam conforme o defeito:
- **Layout, grid, largura**: 375/390, 1366x768, 1440x900, 1920x1080 e a resolução do print do cliente (muitos defeitos só aparecem nela). Conferir com 1, 2 e muitos itens.
- **Scroll**: lista com poucos e com muitos itens; o último item não pode ficar coberto.
- **Camadas**: abrir o elemento perto da borda inferior, dentro de um modal rolado e no celular.
- **Formulário**: enviar vazio, com erro e com sucesso; percorrer só com teclado.
- **Imagens**: foto em retrato, em paisagem e uma bem pequena.
- **Celular**: Safari do iPhone e Android (reais ou emulados), com teclado aberto se houver digitação.

Critério mínimo em todas: nada sobreposto, nenhum texto cortado, nenhuma rolagem horizontal, composição do desktop preservada.

## 4. Menu atrás ou cortado (diagnóstico antes do z-index)

1. **Cortado** na borda do card ou do modal → algum ancestral tem `overflow: hidden/auto`. Z-index não resolve.
2. **Atrás** de outro elemento → algum ancestral cria stacking context: `transform`, `filter`, `backdrop-filter`, `opacity < 1`, `will-change`, `contain`, `mix-blend-mode`, `isolation`, `position: fixed/sticky` ou position com z-index.
3. Resolver na origem, nesta ordem de preferência:
   - top layer nativo: `<dialog>` com `showModal()` para modal, Popover API para menu e tooltip;
   - portal no `body`;
   - escala de z-index em tokens: base 0, dropdown 100, sticky 200, overlay 300, modal 400, popover dentro do modal 450, toast 500. Tudo que abre **a partir de** um modal fica acima dele.
4. Backdrop de modal/drawer: fecha ao clicar fora quando a ação é segura, bloqueia a rolagem do fundo e usa `scrollbar-gutter: stable` para a página não pular.

## 5. Mídia em site

- **Carrossel ou coluna?** Coluna (grade auto-fit) quando a pessoa precisa comparar ou ler tudo: planos, preços, itens. Carrossel no mobile para vitrine que se folheia: fotos, depoimentos, acomodações.
- Carrossel: CSS puro com `scroll-snap-type: x mandatory`, itens ~85% da largura para mostrar a ponta do próximo, `overscroll-behavior-x: contain`, setas e indicadores acessíveis no desktop, sem autoplay (se houver, pausa no toque e no hover). Nada de lib de slider.
- Hero: sem `loading="lazy"`, com `fetchpriority="high"`. Abaixo da dobra: lazy.
- Fallback de formato exige `<picture>` com `<source type>`; um `<img srcset>` só em WebP não é fallback.
- Lightbox: Esc, clique fora e botão fechar visível; setas do teclado e swipe; contador; foco volta à miniatura.
- Ícones: uma biblioteca SVG só, `currentColor`, decorativo com `aria-hidden`. Botão de WhatsApp usa o ícone oficial (casa).

## 6. Pedido para outro executor

Quando quem vai executar é outra IA ou pessoa, entregar assim (curto, para copiar e colar junto com o print):

```
Na tela [nome], [problema e impacto].
Aplique [termo]: [comportamento esperado].
Preserve: [o que está certo].
Desktop: [comportamento]. Mobile: [comportamento].
Não use: [lista da seção 3 + cuidado específico do termo].
Validação: [resoluções e casos da seção 3 para este tipo de defeito]. Informe a causa raiz e os arquivos alterados.
```

Para o cliente, explicar o termo em linguagem do dia a dia ao lado ("sticky: gruda no topo enquanto o resto rola"); o pedido em si pode ter CSS, porque é para o executor.

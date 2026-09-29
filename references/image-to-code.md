# Imagem para código e referência gerada

Destilado de image-to-code-skill, imagegen-frontend-web e imagegen-frontend-mobile (taste-skill). Vale para print, export do Claude Design, Figma ou imagem gerada.

**Casa primeiro**: se existe Claude Design (.jsx) ou Figma, **portar fielmente**; nada de gerar referência nova por cima nem "reinterpretar". Gerar imagem de referência só em projeto sem design **e** com custo aprovado pelo usuário (teto de créditos pagos). Imagem é meio, não entrega.

## 1. Análise obrigatória antes de codar (por seção)
- Texto exato; número de linhas e onde quebra; alinhamento.
- Escala como razão (H1 : sub : body), não px chutado.
- Espaços: título→sub, texto→botão, entre cards, topo e fundo da seção, gutters, padding interno.
- Raio, divisores, forma e padding do botão, hierarquia primário/secundário.
- Paleta em hex amostrado, tratamento de imagem e ícone, sombras.
- Grid, ordem e ritmo das seções, motivos que se repetem.

## 2. Anti-drift
- Não "melhorar" a referência deixando-a genérica.
- Não comprimir espaçamento.
- Não trocar tipografia forte por uma segura.
- Não adicionar pill, stat ou micro-rótulo que não existe no original.
- Board comprimido nunca é fonte de medida: pedir a seção isolada em tamanho maior, não recortar.

**Ambiguidade**, resolver nesta ordem: linguagem visual dominante → layout e espaço → família de componentes → mood → pedir detalhe → escolher a versão mais implementável.

## 3. Mídia
Frame com `aspect-ratio` fixo em todo módulo repetido; mesmo tratamento (grade de cor, crop) entre imagens; fotos distintas por seção (casa). Sem imagem disponível: slot marcado com dimensão e proporção, listado no resumo final. Nada de ilustração SVG improvisada nem `picsum.photos`.

## 4. Gerar referência de página web (quando aprovado)
- Uma imagem horizontal por seção (16:9 ou 16:10), rotulada "Seção X de N". Nunca uma imagem longa com a página inteira.
- Quantidade de seções vem do conteúdo real do cliente, não de um pacote padrão.
- Hero: não cair em texto-à-esquerda/imagem-à-direita por reflexo. Alternativas: centralizado sobre imagem (texto nos 40% inferiores), inferior-esquerdo sobre imagem, stacked center, image-as-canvas com área segura, offset editorial, mini minimalista. Escolher **uma** escala: Giant Statement, Mid Editorial ou Mini Minimalist.
- Por seção, âncora + modo de fundo. Rejeitar o conjunto se a mesma âncora aparece em mais de 2 seções seguidas ou o mesmo fundo em mais de 3.
- Uma espinha narrativa por página (artefato, jornada, instrumento de precisão, sistema vivo, palco, dossiê) e **exatamente um** momento de segunda leitura (sangria assimétrica, numeral estrutural, troca de material, macro crop).
- Intensidade de fundo alterna ao menos 2 vezes; ao menos 2 tipos de crop (macro + contexto). Paleta travada em todas as imagens.
- Proibido: ticker de logos ilegíveis, 3 colunas de KPI idênticas, número de exemplo que vire UI (zero número fake).

## 5. Gerar referência mobile (quando aprovado)
- Decidir a plataforma (iOS, Android ou neutro) e não misturar padrões.
- Travar a "bíblia" antes da tela 2: paleta, tipografia, espaço, raio, ícones, imagem, textura, navegação, botão, sombra.
- Uma imagem por tela, mesmo mockup e mesma escala, margens iguais. Cada tela tem motivo para vir depois da anterior.
- Primeira tela: um ponto focal, título de 1-3 linhas, uma ação, sem chips nem stats. Imagem atrás de texto só com scrim.
- Respeitar safe areas; no máximo 2 níveis de contêiner. Não pode parecer site dentro de celular.
- Texto pequeno demais = tela não terminada: reduzir conteúdo, aumentar espaço ou dividir em outra tela.

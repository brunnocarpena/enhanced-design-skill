# Anti-slop — fallback do Impeccable

Use quando a skill Impeccable **não** estiver instalada. Captura a essência das 46 regras determinísticas de detecção de "slop" (o cheiro de UI gerada por IA). Rodar na fase 4c. Para cada item, apontar arquivo/seletor e sugerir a correção — não só listar.

> Se o Impeccable estiver instalado, prefira `Skill: impeccable` (ou `/impeccable quieter` / `/impeccable audit`). Este checklist é o plano B honesto.

## Sinais de slop (o que enxugar)

### Cor e gradiente
- [ ] **Gradiente roxo/violeta genérico** de fundo ou em CTA sem justificativa da marca → trocar por cor sólida da paleta ou gradiente intencional.
- [ ] **Gradiente em texto de heading** só pra "dar graça" → remover; deixar o peso e o tamanho fazerem o trabalho.
- [ ] Mais de 2 gradientes competindo na mesma dobra → reduzir a 1 foco.
- [ ] Cores neon saturadas sem contraste real com o fundo.

### Tipografia
- [ ] **Peso de fonte pesado (700+) em TODO heading** → hierarquia vira grito; variar peso/tamanho.
- [ ] Fonte batida de template (Inter/Poppins default sem intenção) → escolher par intencional (ver ui-ux-pro-max).
- [ ] Mais de 2 famílias de fonte sem sistema.
- [ ] `text-align: center` em blocos longos de parágrafo.

### Sombra e borda
- [ ] **Sombra de card clichê** (aquela `0 4px 6px rgba(0,0,0,.1)` que todo mundo reconhece) → sombra mais sutil/intencional ou borda fina.
- [ ] **Borda de acento colorida na lateral do card** (o clichê que o Impeccable ensina a reconhecer) → remover ou repensar.
- [ ] Border-radius gigante e inconsistente entre componentes.
- [ ] Glow/box-shadow colorido em tudo.

### Conteúdo e microcopy
- [ ] **Texto genérico placeholder**: "Bem-vindo à nossa plataforma", "Soluções inovadoras para o seu negócio", "Transforme seu X em Y" sem especificidade → reescrever com o assunto real.
- [ ] **Emoji decorativo espalhado** em headings/bullets sem função → remover (emoji só quando carrega significado).
- [ ] "Lorem ipsum" ou números redondos fake ("+1000 clientes", "99% de satisfação") sem dado real → só dado real, ou remover.
- [ ] Ícones aleatórios que não representam o item (o primeiro da lib).

### Layout e movimento
- [ ] Seções todas com o mesmo card-grid de 3 colunas → variar ritmo.
- [ ] Animação de fade-in em absolutamente tudo → movimento intencional, no que precisa saltar.
- [ ] Espaçamento vertical inconsistente entre seções (ver house-rules: padding igual).
- [ ] Hero com stock photo genérica de "equipe sorrindo apontando pra laptop".

## Checagens mecânicas (contar, não achar)

Destiladas da taste-skill e do frontend-design. Rodar com grep/contagem no código; o número decide, não a impressão. Marcadas com (mkt) valem só para página de marketing/landing.

### Estrutura da página (mkt)
- [ ] **Eyebrows** (rótulo pequeno em caixa alta acima do título): no máximo `ceil(seções / 3)`. Contar `uppercase` + `tracking` fora do CTA. Eyebrow numerado ("01 / Método") só se a sequência for real.
- [ ] **Zigzag** texto/imagem alternado: no máximo 2 seções seguidas.
- [ ] Página com 8+ seções usa pelo menos 4 famílias de layout diferentes.
- [ ] **Bento**: número de células = número de itens reais; 2-3 células visualmente diferentes das outras.
- [ ] Cada seção: título ≤ 8 palavras + sub ≤ 25 palavras + 1 visual ou CTA. Seção "despejo de dados" divide.
- [ ] Lista com mais de 5 itens: alternativa a `divide-y` (grade, colunas, agrupamento). Nunca `border-t` E `border-b` em toda linha.
- [ ] Muro de logos = só logos (sem texto de apoio em cada um). No máximo 1 marquee por página.
- [ ] Barra de pontuação com trilha preenchida ("92% de satisfação") → proibida sem dado real.

### Hero (mkt)
- [ ] Headline ≤ 2 linhas no desktop; sub ≤ 20 palavras e ≤ 4 linhas no mobile.
- [ ] No máximo 4 elementos de texto (eyebrow, título, sub, CTA).
- [ ] Proibido dentro do hero: tagline embaixo do CTA, faixa "usado por", teaser de preço, pilha de avatares, selo BETA falso, faixa decorativa no rodapé, badge flutuante, indicador "role para baixo".
- [ ] Tem um visual real (foto, produto, demo), não bloco de div fingindo screenshot.
- [ ] Altura com `100dvh`/`min-h-dvh`, nunca `100vh` (barra do Safari no mobile).

### Nav e rodapé
- [ ] Nav em uma linha, 64-72px de altura (teto 80px).
- [ ] Sem footer com número de versão, "feito com amor", crédito de foto inventado.

### Conteúdo
- [ ] Depoimento ≤ 3 linhas, com nome + papel reais, aspas curvas (" ").
- [ ] Ponto médio (·) no máximo 1 por linha.
- [ ] Rótulo do CTA igual em toda a página para a mesma ação.
- [ ] Um registro de copy só (não misturar formal com gíria).
- [ ] **Zero travessão (U+2014) e meia-risca (U+2013)** em texto user-facing, faixas incluídas ("9h-18h").

### Decoração proibida
- [ ] Faixa de cidade/hora/clima, crédito de foto fake, pill sobre a foto, contador de estoque fake, texto vertical girado 90°, cruzinhas/crosshair decorativas, truque de `<br>` + itálico para "dar ritmo".
- [ ] Cursor customizado. Bolinha pulsante sem estado ao vivo real.
- [ ] Itálico display com descendentes (g, j, p): `line-height ≥ 1.1` + `padding-bottom` 0.25rem, senão corta.

### Tells de calibração (frontend-design)
Se a página cair num destes sem justificativa escrita no design.md, é slop mesmo "bonito": creme + terracota/serif; quase-preto + acento ácido; broadsheet de jornal; kit SaaS (gradiente roxo, Inter, 3 cards de feature com ícone); cromo de template (nav + hero centralizado + logos + 3 features + depoimentos + CTA, nessa ordem). Ver `aesthetic-direction.md` seção 5.

## Como reportar
Para cada achado: **arquivo:linha/seletor → o que é o slop → correção sugerida → severidade (alta/média/baixa)**. Alta = clichê que grita amadorismo na primeira dobra. Baixa = detalhe fora da dobra. Nas checagens mecânicas, informar a contagem ("eyebrows: 5 para 9 seções, teto 3").

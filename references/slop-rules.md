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

## Como reportar
Para cada achado: **arquivo:linha/seletor → o que é o slop → correção sugerida → severidade (alta/média/baixa)**. Alta = clichê que grita amadorismo na primeira dobra. Baixa = detalhe fora da dobra.

# Raciocínio de design — fallback da fase 1

Use quando `ui-ux-pro-max` e/ou `frontend-design` **não** estiverem instaladas. O ponto da fase 1 não é gerar código rápido — é o contrário: **forçar o modelo a raciocinar antes de escrever a primeira linha**, pra o resultado não sair genérico.

> Caminho principal: o motor do ui-ux-pro-max via Bash ([ui-ux-pro-max-engine.md](ui-ux-pro-max-engine.md)) + os 2 passes de [aesthetic-direction.md](aesthetic-direction.md). Este arquivo é o plano B quando o motor falha (Python ausente, script movido) e nenhuma das skills está disponível.

## Antes de qualquer código, decidir explicitamente (busca em 5 domínios)

1. **Produto** — o que é, pra quem, que ação única a página precisa provocar. Que sensação (confiável / sofisticado / enérgico / calmo)?
2. **Estilo** — escolher UM: minimalista, editorial, neo-brutalismo, glassmorphism, corporativo sofisticado, bento... e justificar pelo brief. Assumir **um risco estético** defensável.
3. **Cor** — paleta intencional (primária + neutra + 1 acento), com contraste real. Fugir do gradiente roxo default.
4. **Tipografia** — par de fontes deliberado (display + corpo). Fugir de fonte batida sem intenção. Escala tipográfica com contraste real de peso/tamanho.
5. **Landing/layout** — ritmo das seções (não tudo card-grid de 3 colunas), primeira dobra que entrega a mensagem, movimento só onde precisa saltar.

## Anti-padrões a marcar de saída (o que NÃO fazer)
- Fontes batidas de template sem intenção.
- Esquema de cor "de sempre" / gradiente roxo genérico.
- Sombra de card clichê + borda de acento na lateral.
- Emoji decorativo espalhado.
- Copy genérica ("Bem-vindo à nossa plataforma").
- Hero com stock photo genérica.

## Estética não é sistema
Escolher um estilo (glass, brutalista, editorial) não substitui tokens, estados e escala. Um DS por projeto: se já existe (shadcn, Astryx, o da marca), ele é a fundação e o estilo se aplica por cima.

## Saída da fase 1
Um **design system enxuto** em tokens: cor, tipografia, espaçamento, raio, sombra, movimento, com os anti-padrões acima marcados como proibidos. Registrar em `design.md` do projeto no formato de [design-md-template.md](design-md-template.md). As fases seguintes constroem em cima disso.

**Regra de ouro:** quanto mais específico o brief, melhor o resultado. Design skill não lê sua mente — amplifica sua direção. "Redesign drástico, explore paletas e pares de fonte" volta muito diferente de "faz bonito".

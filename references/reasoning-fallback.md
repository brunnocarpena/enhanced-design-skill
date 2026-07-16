# Raciocínio de design — fallback da fase 1

Use quando `ui-ux-pro-max` e/ou `frontend-design` **não** estiverem instaladas. O ponto da fase 1 não é gerar código rápido — é o contrário: **forçar o modelo a raciocinar antes de escrever a primeira linha**, pra o resultado não sair genérico.

> Se as skills estiverem instaladas, invoque-as (`Skill: ui-ux-pro-max`, `Skill: frontend-design`). Este é o plano B.

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

## Saída da fase 1
Um **design system enxuto** em tokens: cor, tipografia, espaçamento, raio, sombra, movimento — com os anti-padrões acima marcados como proibidos. Registrar em `design.md` do projeto se ele durar mais que a sessão. As fases seguintes constroem em cima disso.

**Regra de ouro:** quanto mais específico o brief, melhor o resultado. Design skill não lê sua mente — amplifica sua direção. "Redesign drástico, explore paletas e pares de fonte" volta muito diferente de "faz bonito".

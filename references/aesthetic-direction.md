# Direção estética (Fase 1)

Destilado da `frontend-design` (versão do plugin oficial) e do núcleo da `taste-skill` (Leonxlnx, commit 87da8a4), filtrado pelas regras da casa. Este arquivo é a fonte da Fase 1; não é preciso carregar as skills originais.

## 1. Design Read (1 linha, antes de qualquer token)

Ler seis sinais do brief: tipo de página, palavras de vibe, referências, público, assets de marca, restrições silenciosas (setor regulado, público idoso, acessibilidade). **Restrição silenciosa vence a estética.**

Declarar:

> Lendo isto como: <tipo de página> para <público>, com linguagem <vibe>, puxando para <estética/DS>. Trabalho principal da página: <1 ação>.

Brief ambíguo = **uma** pergunta, não um questionário. Marca com identidade definida (paleta, fonte, Claude Design) = a marca manda; as regras de "escolha livre" abaixo não se aplicam a ela.

## 2. Dials (registrar no design.md)

Três números de 1 a 10 que viram gatilho de regra:

| Dial | 1-3 | 4-7 | 8-10 |
|---|---|---|---|
| **VARIANCE** (quanto foge do grid previsível) | Simétrico, centralizado | Assimetria pontual no desktop | Composição quebrada, editorial |
| **MOTION** | Só `:hover`/`:active` | Reveal nas seções-chave + entrada do hero | Coreografia de scroll (pin, pan) |
| **DENSITY** | Muito respiro | Padrão | Denso: 1px entre dados, sem card |

Presets da casa:

| Caso | V/M/D |
|---|---|
| LP de captura / lançamento | 5/4/4 |
| LP de venda longa | 5/5/4 |
| Portfólio / agência / demo sitesfixe | 8/7/3 |
| Editorial / blog | 6/4/3 |
| Dashboard / painel interno | 3/2/7 |
| Redesign preservando | dials do site atual, motion +1 |

Gatilhos: MOTION > 3 obriga `prefers-reduced-motion` forte (loop, parallax, pin e magnético viram estáticos). DENSITY > 7 proíbe card como agrupador. **Motion declarado = motion entregue**: dial ≥ 4 (a faixa 4-7 inteira) exige entrada do hero, reveal nas seções-chave e hover no CTA; se não der para entregar sem quebrar, baixar o dial e entregar estático limpo.

## 3. Dois passes: plano, crítica, depois código

**Passe 1 - plano de tokens** (curto, no chat ou no design.md):
- 4-6 cores nomeadas em hex (fundo, superfície, texto, texto secundário, acento, e CTA se for decisão da marca).
- Famílias tipográficas e o papel de cada uma (1-2 famílias, claramente distintas: categorias diferentes, como serifa x grotesca x mono; duas grotescas parecidas contam como uma só. Uma família só vale, com papéis separados por peso/estilo).
- Conceito de layout em wireframe ASCII da primeira dobra e de 2-3 seções, com o eixo de alinhamento.
- 2-3 princípios ("um único momento de motion no hero", "números em tabular-nums").

**Passe 2 - crítica**: reler o plano contra o brief e contra a lista de defaults (seção 5). Tudo que ficou genérico é revisado, e **dizer o que mudou** ("troquei o creme+terracota por floresta+âmbar porque..."). Só então codar.

## 4. Princípios de direção

- **Chão no assunto**: nomear o assunto concreto, o público e o trabalho principal. Design sem assunto vira decoração.
- **Hero abre com o mais característico** do assunto. "Número grande + rótulo pequeno + stats + acento em gradiente" é o default de template.
- **Tipografia**: 1-2 famílias, escala com contraste real (Elements of Typographic Style). Linha de leitura < 80 caracteres no próprio parágrafo; corpo em serifa pede um pouco mais de line-height. Ênfase dentro da headline = itálico ou negrito da **mesma** família, nunca uma palavra em outra família ou outra cor. Serifa não é default de "premium": só quando a marca nomeia ou a estética é de fato editorial/luxo.
- **Estrutura é informação**: 01/02/03 só para sequência real (passo numerado funcional é permitido; rótulo decorativo "SEÇÃO 04" não).
- **Motion**: um momento orquestrado vale mais que efeitos espalhados. Fade-and-slide em toda seção e hover em todo card lê como IA. Motion que responde a uma ação do usuário é bem-vindo. Toda animação responde "o que comunica?"; "ficou legal" corta.
- **Contenção**: gastar a ousadia em **um** lugar. Antes de entregar, tirar um acessório (Chanel).
- **Estética não é sistema**: glass, bento, brutalismo, aurora, kinetic type não têm pacote oficial; é aproximação, e o código diz isso em comentário. Se o projeto usa um DS (shadcn, etc.), usar o pacote de verdade, **um DS por projeto**, sem importar tokens para sobrescrever 90% deles.

## 5. Defaults de calibração (evitar, salvo se o brief pedir)

São as escolhas que o modelo faz sozinho. Se o plano caiu numa delas, o Passe 2 revisa:

1. **Creme + serifa display + terracota** (#F4F1EA, #D97757). Variante "premium consumer": bege/creme (#f5f1ea, #faf7f1, #efeae0) + latão/argila/oxblood (#b08947, #b6553a, #9a2436) + texto espresso (#1a1714). Alternativas: prata fria, floresta + âmbar, preto + tan, cobalto + creme, terracota + ardósia, oliva + tijolo, monocromo + 1 pop.
2. **Quase-preto com um acento ácido** (verde-limão ou vermelhão).
3. **Broadsheet**: filetes finos, raio zero, colunas densas.
4. **Kit SaaS**: cards idênticos arredondados, mesmo raio em tudo, sombra `rgba(0,0,0,.1)`, lavagem de gradiente decorativa.
5. **Cromo de template**: eyebrow em caixa alta com tracking em todo título, strings "A · B · C", rótulo "PALAVRA - fragmento", #0B0B0B/#111 em vez de preto pensado, mono para rótulo pequeno de dado, "→" colado no botão.
6. Gradiente roxo/violeta genérico; Inter/Poppins sem intenção; Fraunces/Instrument Serif como escolha automática.

**Casa x frontend-design**: o CTA da casa é CAIXA ALTA + shimmer por decisão de marca. Isso **não** conta como eyebrow nem como "caixa alta de template". A regra anti-caixa-alta vale para rótulos e eyebrows.

## 6. Cor, forma e material (escolha livre)

- 1 cor de acento, saturação < ~80%; neutros de uma só temperatura. Trava de acento na página inteira. CTA de cor diferente por decisão da marca não é violação. Os pares da seção 5 ("floresta + âmbar") são 1 acento + 1 neutro de apoio, não 2 acentos; latão, âmbar e ouro ocupam o mesmo papel.
- Sem #000 e #fff puros como superfície; off-black e off-white.
- Sombra tingida com o matiz do fundo; sem glow/neon como default.
- **Trava de raio**: uma escala na página (tudo 0, tudo 12-16px, ou pílula nos interativos). Sistema misto só com regra escrita e seguida em todo lugar.
- Card só quando a elevação comunica hierarquia; senão agrupar com espaço, `border-top` ou `divide-y`.
- Glass: borda interna 1px `rgba(255,255,255,.1)` + `inset 0 1px 0 rgba(255,255,255,.1)` e fundo sólido em `@media (prefers-reduced-transparency: reduce)`; o contraste funciona sem blur.
- Grain/noise só em pseudo-elemento `position: fixed; inset: 0; pointer-events: none`, nunca em container que rola.
- Tema: decidir na Fase 1 (claro, escuro ou auto) e registrar. Um tema por página; nenhuma seção inverte claro/escuro no meio (exceção: 1 troca deliberada).

## 7. Escrita de interface

- Do lado do usuário, linguagem simples, voz ativa ("Salvar alterações", não "Enviar").
- Mesmo nome da ação no fluxo inteiro ("Publicar" gera "Publicado"). Um rótulo de CTA por intenção na página: repetir o mesmo em nav, hero e rodapé.
- Erro não pede desculpa e nunca é vago: diz a causa e como corrigir.
- Estado vazio é convite para agir (mensagem + ação).
- Sentence case, sem enchimento, um trabalho por elemento. Um registro de copy por página.
- Verbos de enchimento proibidos: "Eleve", "Revolucione", "Desbloqueie", "Potencialize", "Sem esforço", "Next-level", "Transforme seu X".
- Rótulo poético de artesão proibido ("Notas de campo", "Na bancada"). Rótulo funcional ou nenhum.
- Autoauditoria: reler toda string visível (títulos, alt, rodapé, erros) e reescrever frase quebrada, referente solto ou trocadilho sem sentido. Copy fofa de IA é pior que copy sem graça.

## 8. Piso de qualidade (não negociável)

Responsivo mobile-first, foco visível, reduced-motion, acessível, cores harmônicas, e CSS sem classes se cancelando (conferir especificidade: `.section` x `.cta` brigando por padding). Autocrítica com screenshot real antes de entregar.

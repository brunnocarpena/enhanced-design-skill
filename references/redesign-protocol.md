# Protocolo de redesign (modo refine)

Destilado da redesign-skill e da seção 11 da taste-skill. Princípio: **atualizar em cima da stack que existe**. Nunca reescrever do zero nem migrar framework.

## 0. Detectar o submodo
- **Preservar**: a marca e a estrutura ficam; elevar acabamento. Dials = os do site atual, motion +1.
- **Overhaul**: direção nova com ok explícito. Roda a Fase 1 completa.
- Porte de Claude Design / Figma **não é redesign**: é porte fiel (regra da casa). Mudar só o que foi sinalizado.

## 1. Scan (antes de mexer)
- `package.json`: o que já está instalado (nada de import alucinado). Tailwind v3 (`tailwind.config`) ou v4 (`@theme` no CSS). Método de estilo (Tailwind, CSS modules, styled).
- `design.md`/`DESIGN.md` do projeto, se existir.
- Tokens da marca extraídos **antes** de qualquer regra de cor. Marca roxa continua roxa.
- Baseline de SEO: páginas que ranqueiam, títulos, JSON-LD, OG. É o risco nº 1 de redesign.
- Leitura dos dials do site atual (V/M/D).

## 2. Nunca muda sem ok explícito
Slugs e URLs; rótulos da nav principal; **nome e ordem dos campos de formulário** (quebra analytics e autofill); IDs de seção e eventos usados pelo tracking/UTM; logo; textos legais e de consentimento. Não regredir a acessibilidade existente (foco, alt, teclado, contraste).

## 3. Diagnóstico
Rodar `seek-patterns` + `slop-rules.md` + os estados faltantes. Listar antes de corrigir.

## 4. Corrigir nesta ordem (parar quando o brief estiver satisfeito)
Um diff revisável por passo, conferido depois de cada um:

1. **Tipografia**: trocar família batida sem intenção; pesos 500/600; escala consistente.
2. **Espaçamento e ritmo**: padding de seção único; grid auto-fit no lugar de flex com %; raio concêntrico; botões alinhados no rodapé dos cards.
3. **Cor**: um acento; cinza de uma família; tirar #000 puro; tirar seção escura aleatória no meio de página clara.
4. **Estados**: hover, active e focus em todo clicável; skeleton no lugar de spinner de conteúdo; estado vazio composto; erro inline; nada de `window.alert`.
5. **Motion**: um momento orquestrado; cortar fade em tudo.
6. **Recompor o hero ou a seção-chave.**
7. **Trocar bloco genérico**: card só quando elevação é hierarquia; pricing destaca plano por cor, não por altura; sheet ou inline no lugar de modal para tudo; FAQ fora de card; alternativa ao carrossel de 3 depoimentos (mural de citações).

## 5. Varredura de omissões
Link legal no rodapé, página 404, skip-link, nav ativa marcada, favicon, validação de formulário, `href="#"` (é bug), consentimento de cookies quando houver tracking.

## 6. Varredura de conteúdo
Nada de "Acme"/"João da Silva"/avatar-ovo em demo: nomes críveis do nicho. Sem "Oops", sem exclamação em sucesso. Voz ativa, sentence case (caixa alta só no CTA). Clichês fora: "Eleve", "Revolucione", "Desbloqueie", "Sem esforço", "solução poderosa", "Next-level".

## 7. Upgrades opcionais (só com espaço e pedido)
Variable font; ícone outline → fill no hover; broken grid; cards empilhados com sticky; split scroll; spotlight border; glass com borda interna; sombra tingida no acento.

## 8. Fechar
Fases 4a → 4d e o gate. No resumo final, listar o que mudou por alavanca e o que ficou intocado de propósito.

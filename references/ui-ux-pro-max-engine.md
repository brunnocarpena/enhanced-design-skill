# Motor da ui-ux-pro-max (chamada direta)

Em vez de carregar o SKILL.md da ui-ux-pro-max (43 KB), a Fase 1 chama o motor de busca dela direto. É Python só com biblioteca padrão (python3 do sistema serve), responde em ~0,07s.

```bash
S=~/.claude/skills/ui-ux-pro-max/scripts/search.py
cd ~   # sempre rodar de $HOME (Documents dá EPERM)
```

Se o arquivo não existir, cair no `reasoning-fallback.md` e dizer isso no resumo final.

## Limitação que manda em tudo

O motor é **BM25 por palavra-chave**, não entende semântica, e os CSVs estão em inglês. Consequências:

- A query vai **em inglês**, com o vocabulário da coluna Product Type: "mental health", "wellness", "spa", "beauty", "coaching", "course", "luxury", "fintech", "saas", "restaurant"...
- Palavras genéricas ("landing", "course", "app") puxam a categoria errada.
- A saída é **proposta a validar contra o brief**, nunca decisão. Estilo energético para público calmo = rejeitar e re-rodar.

## Protocolo (Fase 1)

1. Traduzir o brief para 3-6 palavras em inglês do vocabulário acima.
2. Conferir a categoria: `python3 $S "<palavras>" --domain product -n 3`. Se o 1º resultado não é o produto certo, trocar as palavras antes de seguir. Teto de 4 tentativas: depois disso, usar a categoria mais próxima e registrar no design.md que é aproximação (nicho sem categoria no CSV).
3. Design system: `python3 $S "<palavras da categoria certa>" --design-system -f markdown -p "<Projeto>"`.
4. Validar Pattern, Style, Colors, Typography e Avoid contra o tom do brief. Divergiu? Re-rodar ou buscar alternativa em `--domain style`.
5. Aprofundar só o que ficou em dúvida: `--domain color|typography|google-fonts|landing|chart|icons`.
6. Stack: `python3 $S "<tema>" --stack nextjs|react|html-tailwind|shadcn`. Greenfield sem stack decidida: usar `nextjs` (padrão da casa); projeto que não é web, pular.
7. Passe de UX antes de codar: `python3 $S "animation accessibility z-index loading" --domain ux` (+ "form" ou "navigation" conforme a tela).
8. Consolidar tokens filtrados pelas regras da casa (seção abaixo) e registrar em **um** lugar só: `design.md` do projeto (padrão) **ou** `--persist`.

## Modos e flags

| Modo | Comando |
|---|---|
| Design system | `--design-system [-p Nome] [-f ascii\|markdown]` (padrão ascii; usar markdown) |
| Persistir | `--design-system --persist -p Nome [--page checkout] -o "/caminho/abs/do/projeto"` |
| Domínio | `"<palavras>" --domain <d> [-n N] [--json]` (padrão n=3) |
| Stack | `"<palavras>" --stack <s> [-n N]` |

- `--persist` **sem `-o` grava no diretório atual** (sujaria `~/design-system/`). Sempre `-o` absoluto.
- Domínios válidos: `product`, `style`, `color`, `typography`, `google-fonts`, `landing`, `ux`, `chart`, `icons`, `react`, `web`.
- O SKILL.md original cita um domínio `prompt`: **não existe** (`invalid choice`).
- Stacks: react, nextjs, vue, svelte, astro, swiftui, react-native, flutter, nuxtjs, nuxt-ui, html-tailwind, shadcn, jetpack-compose, threejs, angular, laravel. Ignorar a frase "React Native (this project's only tech stack)" do SKILL.md original: é resíduo do autor.

## Filtro da casa sobre a saída (a casa sempre vence)

| O motor sugere | A casa faz |
|---|---|
| `max-w-6xl/7xl` no container | Página sem max-width; respiro por padding lateral |
| Medida 65-75 caracteres / `max-w-prose` | Limite só no próprio parágrafo, nunca bloco estreito solto (`flex-1 min-w-0`) |
| `truncate`, `line-clamp`, reticências + tooltip | Nunca cortar: `overflow-wrap: anywhere` ou o card cresce |
| `grid-cols-2/3` por breakpoint | `repeat(auto-fit, minmax(min(100%, var(--grade-min)), 1fr))`; 375/768/1024/1440 são só pontos de teste |
| Estilo com "border-left accent" | Proibido card com borda de um lado só |
| Títulos 600-700 | Variar peso e tamanho; não 700 em tudo |
| "animated patterns, scroll-snap, bold hover", Vibrant/Claymorphism, "Avoid: muted colors" | Vêm da categoria, não do brief. Filtrar pelo tom real e pelo `slop-rules.md` |
| Estatísticas de conversão ("3x time-on-page") | Heurística interna. **Nunca** viram copy nem número na UI |
| CTA accent ao fim de cada capítulo + sticky | Posição pode usar; o CTA segue a casa: CAIXA ALTA + shimmer, centralizado, largura total com teto ~460px, 1 linha, ícone oficial do WhatsApp quando for WhatsApp |
| "—" nos textos gerados | Hífen em todo texto user-facing |
| Checklists de app nativo (haptics, tab bar iOS, hitSlop) | Na web, só o equivalente: safe-area, alvo 44px, feedback de pressão |
| "skeleton/shimmer" de loading | É outro shimmer, não o do CTA. Discreto |

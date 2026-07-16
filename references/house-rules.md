# Padrões da casa (Brunno / BMC Mídia)

Acabamentos cobrados diretamente pelo usuário ao longo dos projetos. Use quando `seek-patterns` não estiver disponível (a skill tem o catálogo completo em `~/.claude/skills/seek-patterns/CATALOG.md`). Aplicar por padrão em toda UI — sem esperar o usuário apontar um a um.

## CTA
- **CAIXA ALTA + shimmer**; nunca quebrar em 2 linhas.
- **Centralizado, largura total**, com teto de largura (~460px).
- Botão com destino WhatsApp usa o **ícone oficial do WhatsApp**.

## Layout e primeira dobra
- **Primeira dobra visível**: o essencial da hero cabe sem rolar.
- **Espaçamento vertical consistente**: mesmo padding entre todas as seções.
- **Mobile-first sempre**: ajustar no mobile antes do desktop.

## Tipografia e texto
- **Sem palavra órfã** na última linha (usar `text-wrap: balance`/`pretty`).
- **Sem em-dash** em texto user-facing: usar hífen "-", nunca "—"/"–". Varrer com perl/grep antes de subir.
- Copy congruente com o produto real (nada de benefício sem código por trás).

## Imagens
- **Não repetir a mesma foto** da pessoa (ex: Mari) em seções diferentes — fotos distintas por seção.
- Imagem do hero é representativa da própria página.

## Dados na UI
- **Zero métrica fixa/fake**: tudo dinâmico, some quando 0. Nada de "+1000 clientes" hardcoded.

## Formulários (UX)
- Senha com **olhinho** (toggle de visibilidade).
- **Erro inline por campo**, não alerta genérico.
- Sem spinner numérico feio; input adequado ao dado.
- CTA de submit com **estado de envio** (loading/disabled).

## Metadata (toda página nova)
- **title = H1** da página (+ marca quando couber).
- **description = subtítulo/sub-headline** (incluir data do evento quando houver).
- **og:image / twitter:image = imagem da própria página**, nunca de outra campanha.
- Conferir og:title / og:description (espelham title/description).
- Tamanhos: title ≤ ~60 caracteres, description ≤ ~155.
- **Nunca herdar/reaproveitar metadata de outra página** — variante com H1 diferente = metadata diferente.

## Fidelidade de design
- Ao portar de referência (.jsx da Claude Design / Figma), portar **fielmente** — mudar só o que foi sinalizado.

## Fluxo de front-end
- Tarefa de front-end/UI de peso: despachar com as skills de design ativas e cumprir o checklist de refino (cursor-pointer, proporção de popup, espaçamento, copy congruente).

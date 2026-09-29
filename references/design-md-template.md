# Modelo de design.md (saída da Fase 1)

Formato do Stitch (taste-skill), adaptado à casa. Escrever em linguagem natural com o valor entre parênteses ("cantos generosamente arredondados (16px)", não "rounded-xl"), para servir a qualquer stack e a qualquer agente. Se o projeto já tem `design.md`/`DESIGN.md`, atualizar o existente em vez de criar outro.

```markdown
# Design - <Projeto>

## 0. Leitura e dials
Lendo isto como: <tipo de página> para <público>, com linguagem <vibe>. Trabalho principal: <ação>.
- Variance: N - <1 linha de justificativa>
- Motion: N - <justificativa>
- Density: N - <justificativa>
- Tema: claro | escuro | auto

## 1. Atmosfera
Parágrafo curto de mood, com a metáfora ou espinha narrativa.

## 2. Paleta e papéis
- Nome descritivo (#HEX) - papel. Ex.: Tinta Carvão (#18181B) - texto primário.
- No máximo 1 acento; CTA separado só se for decisão da marca.

## 3. Tipografia
Família por papel (display, corpo, mono se houver), escala em rem/clamp, tracking, line-height, pesos permitidos, fontes proibidas.

## 4. Componentes
Botões, cards, inputs, nav, badges e estados, em palavras com valor entre parênteses. Escala de raio única.

## 5. Hero
Arquitetura, escala (giant/mid/mini), âncora visual, 1 CTA primário. Sem "role para explorar", sem chevron.

## 6. Layout
Grade auto-fit com `--grade-min`, gaps, padding lateral, padding de seção único, regra de sobreposição.

## 7. Responsivo
Mobile-first. Testar em 375, 768, 1024 e 1440 + paisagem. Alvos de 44px, corpo >= 16px, sem scroll horizontal.

## 8. Motion
Easing nomeado, durações, stagger, o que anima e o que nunca anima, comportamento em prefers-reduced-motion.

## 9. Anti-padrões deste projeto
Lista explícita do que é proibido aqui (inclui os defaults de calibração evitados no Passe 2).

## 10. Regras da casa aplicadas
CTA CAIXA ALTA + shimmer (teto ~460px, 1 linha); metadata própria (title = H1); grade auto-fit; sem max-width de página; mobile com ícone em cima do texto; zero número fake; sem em-dash.
```

Os dials são vocabulário de decisão, não configuração que vence o brief.

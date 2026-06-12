---
name: Bug
about: Comportamento incorreto ou regressão em relação ao baseline de §1
title: "fix: "
labels: bug
assignees: ""
---

## Descrição do problema
<!-- O que acontece vs. o que deveria acontecer. -->

## Passos para reproduzir
1.
2.
3.

## Comportamento esperado
<!-- Conforme §1 (baseline) ou §3 (invariantes). -->

## Comportamento atual

## Rastreabilidade
<!-- Seção do GUIA-DE-REFATORACAO.md ou issue original que introduziu o comportamento. -->
- Seção/issue relacionada:

## Definition of Ready (DoR)
- [ ] Escopo descrito e rastreável (reprodução confirmada ou seção do guia afetada).
- [ ] Critérios de aceite explícitos (o que confirma a correção).
- [ ] Invariantes de §3 que **não** podem ser tocados identificados.
- [ ] Pontos de permissão de §14, se houver, **já decididos**.
- [ ] Plano de teste (regressão / E2E) definido.

## Critérios de aceite
- [ ] <!-- critério verificável 1 — o bug não ocorre mais -->
- [ ] <!-- critério verificável 2 — nenhuma regressão introduzida -->

## Definition of Done (DoD)
- [ ] Código corrige o comportamento sem mudanças não autorizadas de design/arquitetura.
- [ ] Testes de contrato verdes (sem regressão de API).
- [ ] E2E Playwright cobrindo o cenário corrigido; cobertura de mutação dentro da meta (§11).
- [ ] Commits em Conventional Commits referenciando `#<id>` desta issue (§7).
- [ ] Tema claro visualmente inalterado (quando aplicável).
- [ ] Prompts/skills registrados em `documentacao/` (§17).

## Ambiente
- Node:
- Navegador:
- Branch:

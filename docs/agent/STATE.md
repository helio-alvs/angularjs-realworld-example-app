# STATE

State: REVIEWING
Current phase: 1 — Fundação
Last completed phase: none
Active work unit: docs/agent/work/001-fundacao-infra

## Briefing — o que o próximo agente faz primeiro
1. Ler `docs/agent/OPERATING-GUIDE.md` e este `STATE.md`.
2. Atuar como **closer** (retomada): a slice 001-fundacao-infra está bloqueada por pending_decision.
3. **Pending decision (usuário deve responder antes de prosseguir):**
   - Issues estão **desabilitadas** no repositório (`helio-alvs/angularjs-realworld-example-app`).
   - `gh issue create` retornou erro: `the repository has disabled issues`.
   - **Usuário deve escolher:**
     - **Opção A (recomendada):** Habilitar Issues em Settings → General → Features → Issues, então confirmar o `#<id>` da issue criada.
     - **Opção B:** Confirmar outro mecanismo de rastreamento (Projeto GitHub, PR, ou waiver explícito do requisito de issue).
4. Com o `#<id>` (ou waiver) em mãos, o closer deve:
   - Commitar os artefatos (`.devcontainer/devcontainer.json`, `.github/ISSUE_TEMPLATE/`, `docs/`) com mensagem Conventional Commits referenciando `#<id>`.
   - Confirmar que nenhum arquivo proibido (§13) está staged/tracked.
   - Atualizar PROGRESS.md com a linha do commit.
   - Atualizar STATE.md: state → READY, avançar para a próxima slice da Fase 1.
5. Próxima slice da Fase 1 (após declarar READY): **002-conventional-commits** — configurar Conventional Commits (commitlint/husky) + bleeding branch (§5.2/§7).

## Não faça
- Não inventar requisitos de produto.
- Não escolher stack sem contexto (especialmente os pontos de permissão listados
  no CHARTER.md — Signals vs RxJS, SSR, hashbang vs history, lib markdown).
- Não escrever código de produto antes de uma unidade de trabalho ser planejada.
- Não usar segredos/credenciais/dados sensíveis reais.
- Não commitar `documentacao/`, `harness/`, `AGENTS.md` ou `CLAUDE.md`.

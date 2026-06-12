# STATE

State: READY
Current phase: 1 — Fundação
Last completed phase: none (Fase 1 in-progress — 3/5 itens concluídos)
Active work unit: none (próxima: 002-conventional-commits)

## Slice concluída
- `docs/agent/work/001-fundacao-infra` — **READY**
  - Commit: `a0e1594` (`chore: add issue templates, update devcontainer to Node 22, add agent docs (#1)`)
  - Issue: https://github.com/helio-alvs/angularjs-realworld-example-app/issues/1

## Briefing — o que o próximo agente faz primeiro
1. Ler `docs/agent/OPERATING-GUIDE.md` e este `STATE.md`.
2. Atuar como **planner** para a próxima slice da Fase 1: **002-conventional-commits**.
3. Escopo da slice 002:
   - Configurar **Conventional Commits** no repositório:
     - `commitlint` + `@commitlint/config-conventional`
     - `husky` (hook `commit-msg`)
   - Criar a **bleeding branch** (`bleeding` ou `next`) a partir de `master`.
   - Registrar a configuração em `docs/agent/work/002-conventional-commits/PLAN.md`.
4. Criar issue no GitHub usando o template `chore.md` **antes** de iniciar o trabalho (§6/§7).
5. **Pré-condição a verificar** no PLAN.md da slice 002: confirmar que o repositório tem Issues habilitadas (já confirmado) e que `gh` CLI está disponível (já confirmado — v2.94.0).

## Não faça
- Não inventar requisitos de produto.
- Não escolher stack sem contexto (especialmente os pontos de permissão listados
  no CHARTER.md — Signals vs RxJS, SSR, hashbang vs history, lib markdown).
- Não escrever código de produto antes de uma unidade de trabalho ser planejada.
- Não usar segredos/credenciais/dados sensíveis reais.
- Não commitar `documentacao/`, `harness/`, `AGENTS.md` ou `CLAUDE.md`.

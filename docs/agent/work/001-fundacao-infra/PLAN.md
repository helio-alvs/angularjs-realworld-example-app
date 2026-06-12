# PLAN — 001-fundacao-infra

## Status
State: PLANNED

## Objetivo
Criar os templates de issue GitHub (DoR/DoD conforme §6), atestar o `.git/info/exclude`
já configurado e atualizar o DevContainer para Node LTS (≥18) compatível com Angular CLI v21.

## Escopo
- Incluído:
  - Templates de issue em `.github/ISSUE_TEMPLATE/` (feature, bug, chore — DoR + DoD conforme §6)
  - Verificação/atestação do `.git/info/exclude` (itens do §13 já presentes)
  - Atualização de `.devcontainer/devcontainer.json`: Node `8` → `lts`
- Fora de escopo:
  - Configuração de Conventional Commits (commitlint/husky) — slice separada da Fase 1
  - Criação de bleeding branch — slice separada da Fase 1
  - Qualquer código de produto Angular 21
- files_owned:
  - `.github/ISSUE_TEMPLATE/`
  - `.devcontainer/devcontainer.json`
  - `.git/info/exclude` (somente verificação — não versionado)

## Origem e fase
- Fase (ROADMAP): 1 — Fundação
- Documento/bloco de origem: `GUIA-DE-REFATORACAO.md` §6, §13, §15; `docs/agent/STATE.md`
- context_sources:
  - skills: []
  - design: null
- affects_design_system_core: false

## Dados, segurança e compliance
- Dados envolvidos / sensibilidade: nenhum — arquivos de configuração e markdown de processo
- Permissões / minimização / mascaramento: N/A
- Auditoria / log (sem PII, sem secrets): N/A
- Revisão humana: não requerida (risco B, sem auth/secrets/dados sensíveis)

## API, entidades e integrações
- Endpoints/contratos: nenhum
- Entidades/tabelas: nenhuma
- Integrações externas: nenhuma (GitHub CLI usará templates após criação local)
- Jobs/eventos: nenhum

## Critérios de aceite
- [ ] `.github/ISSUE_TEMPLATE/feature.md` criado com checklist DoR + DoD do §6.
- [ ] `.github/ISSUE_TEMPLATE/bug.md` criado com checklist DoR + DoD do §6.
- [ ] `.github/ISSUE_TEMPLATE/chore.md` criado com checklist DoR/processo do §6.
- [ ] `git status` não exibe `src/documentacao/`, `harness/`, `AGENTS.md`, `CLAUDE.md`
      (verificação do `.git/info/exclude` — atestação, sem alteração necessária).
- [ ] `.devcontainer/devcontainer.json` aponta Node `lts` (ou `22`) — `"version": "lts"`.
- [ ] Documentação e riscos atualizados (`STATE.md`, `PROGRESS.md`).

## Operational path & risk
- risk_category: B   # gatilho: `touches_build_deploy` (devcontainer.json — ambiente de build/dev)
- operational_path: standard_path_B

## Model Profile
```yaml
risk_category: B
planner:
  tier: standard
  effort: medium
  rationale: Planejamento de slice de infra/config com múltiplos arquivos a criar.
generator:
  tier: light
  effort: medium
  rationale: Edições mecânicas de JSON e markdown; nenhum código de produto.
evaluator:
  tier: standard
  effort: low
  rationale: Verificar conformidade dos templates com §6 e validade do JSON; simples mas requer atenção ao guia.
reviewer:
  tier: light
  effort: low
  rationale: Revisão de config/markdown sem superfície de segurança.
cross_family_evaluator: false
budget_max_usd: 0.50   # ~4 turns * $0.08 médio * 1.5
rationale: |
  Risco B pelo devcontainer.json (touches_build_deploy). Demais artefatos são
  markdown de processo (risco A). Tiers light/standard suficientes — nenhuma
  primitiva de segurança ou dado sensível envolvido. Slice pequena e reversível.
```

## Deterministic gates (a rodar antes de READY)
- **schema_validation** — validar JSON do `devcontainer.json` (jsonlint ou `node -e JSON.parse`)
- **config_review** — revisar `devcontainer.json` antes de aplicar (Node version ≥ 18 compat. Angular CLI v21)
- **secret_scan** — confirmar que nenhum segredo foi incluído nos templates ou configs
- **lint** — validar frontmatter YAML dos issue templates (yamllint ou revisão manual)

## Condições de parada
- Versão de Node LTS a usar for incompatível com Angular 21 (verificar docs oficiais antes de alterar)
- O formato de templates de issue do GitHub mudar e o frontmatter exigir campos não documentados
- Qualquer ambiguidade nova sobre conteúdo de DoR/DoD não coberta pelo §6

## Riscos e pendências
- Risco: ~~versão Node `lts` pode resolver para Node 23 (odd) no futuro~~ — **decidido: `"22"` (Node 22 LTS, ativo até 2027)**
- Pendência: Conventional Commits e bleeding branch — itens da Fase 1 não cobertos por esta slice; planejamento separado necessário

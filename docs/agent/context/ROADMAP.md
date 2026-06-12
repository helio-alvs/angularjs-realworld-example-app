# ROADMAP — Conduit Frontend (Angular)

> Fonte: §15 do `GUIA-DE-REFATORACAO.md`. A ordem das fases 1 e 2 é obrigatória
> (a rede de segurança deve preceder a migração). As demais podem variar, exceto
> onde há dependência declarada.

---

## Fase 1 — Fundação
**Objetivo:** Estabelecer a base de governança e infra antes de qualquer código de produto.

**Escopo:**
- ~~Criar templates de Issues no GitHub (DoR/DoD conforme §6 do guia)~~ ✅ **DONE** — slice 001-fundacao-infra (commit `a0e1594`, issue #1)
- ~~Atualizar `.devcontainer/devcontainer.json` para Node LTS compatível com Angular 21~~ ✅ **DONE** — Node 22 LTS (slice 001-fundacao-infra)
- ~~Adicionar `.git/info/exclude` com `documentacao/`, `harness/`, `AGENTS.md`, `CLAUDE.md` (§13)~~ ✅ **DONE** — atestado na slice 001-fundacao-infra (pré-existente)
- Configurar Conventional Commits (§7) — **pendente** (002-conventional-commits)
- Configurar bleeding branch — **pendente** (002-conventional-commits ou slice separada)

**Fora do escopo desta fase:** qualquer código de produto Angular 21.

**Critérios de aceite:**
- [x] Templates de issue criados e usáveis
- [x] `.git/info/exclude` configurado (items do §13 não aparecem em `git status`)
- [x] DevContainer usando Node LTS (build Angular CLI funciona no container)
- [ ] Conventional Commits configurado (commitlint/husky)
- [ ] Bleeding branch existindo no repositório

**Status:** `IN_PROGRESS` — 3/5 itens concluídos; aguardando slice 002-conventional-commits

---

## Fase 2 — Rede de segurança
**Objetivo:** Fixar o comportamento atual antes de reescrever, para que qualquer
regressão seja detectável.

**Escopo:**
- Documentar o contrato da API RealWorld consumida em **OpenAPI 3** (§10)
- Testes de contrato contra a API (§11)
- E2E Playwright mínimos contra o app AngularJS **atual** cobrindo fluxos principais:
  login/registro, listar/abrir artigo, criar/editar artigo, favoritar, seguir

**Fora do escopo desta fase:** migração para Angular 21.

**Critérios de aceite:**
- Documento OpenAPI 3 presente e válido, cobrindo todos os endpoints usados
- Testes de contrato verdes
- E2E Playwright verdes contra o app AngularJS atual

**Dependência:** deve preceder a Fase 3 (obrigatório).

**Status:** `PLANNED`

---

## Fase 3 — Migração Angular 21
**Objetivo:** Reescrever o frontend em Angular 21 com paridade funcional e visual exata.

**Escopo:**
- Criar projeto Angular 21 com Angular CLI
- Reproduzir todas as telas, rotas e fluxos (§3.2)
- Manter contrato de API exato (mesmos paths, headers `Token`, envelopes)
- Manter design system visual pixel-equivalente no tema claro

**Fora do escopo desta fase:** dark mode, rascunhos (são Fase 4).

**Pontos de permissão (resolver antes de iniciar):**
- Signals vs RxJS/serviços
- Hashbang vs history routing
- SSR (`@angular/ssr`) sim ou não
- Biblioteca markdown substituta

**Critérios de aceite:**
- App builda com Angular CLI sem erros
- Testes de contrato (Fase 2) verdes contra o app migrado
- E2E Playwright (Fase 2) verdes contra o app migrado
- Revisão visual: tema claro idêntico ao baseline

**Status:** `PLANNED`

---

## Fase 4 — Incrementos: dark mode e rascunhos
**Objetivo:** Adicionar as duas funcionalidades autorizadas (§4.2 e §4.3).

### 4a — Dark mode
- CSS custom properties + classe `theme-dark` no `<body>`
- Toggle no header/nav (sem reorganizar o header)
- Persistência em `localStorage['conduit.theme']`
- Sem regressão visual no tema claro

### 4b — Sistema de rascunhos
- Autosave com debounce no editor de novo artigo
- Chave `localStorage['conduit.draft.new']` (namespaceada)
- Carregar rascunho ao abrir editor; limpar ao publicar com sucesso
- Ponto de permissão: cobrir edição de artigo existente (`conduit.draft.<slug>`)

**Critérios de aceite:**
- Alternância clara↔escuro funcional e persistente
- Dark mode coberto por E2E
- Rascunho restaurado após reload; limpo após publicação
- Rascunhos cobertos por E2E

**Status:** `PLANNED`

---

## Fase 5 — Qualidade
**Objetivo:** Atingir a meta de mutação e garantir a rede de testes completa.

**Escopo:**
- StrykerJS: meta **95% de mutation score** na base migrada
- Ampliar E2E: dark mode + rascunhos + edge cases
- Branches passa/quebra: manter uma branch com teste Playwright falhando e uma passando

**Status:** `PLANNED`

---

## Fase 6 — Operação
**Objetivo:** Containerização, observabilidade e validação automatizada.

**Escopo:**
- Dockerfile para o app migrado (Nginx estático ou Node SSR — depende de §4.1)
- docker-compose orquestrando frontend + LGTM (Loki, Grafana, Tempo, Mimir)
- Script Python: sobe via Compose, valida logs de inicialização e encerramento
- Observabilidade: mapeamento a definir (ponto de permissão §8)

**Status:** `PLANNED`

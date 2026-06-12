# CHARTER — Conduit Frontend (Angular)

## Identidade
**Nome:** Conduit Frontend
**Objetivo:** Reimplementação do [RealWorld](https://github.com/gothinkster/realworld)
— uma aplicação de blog estilo Medium. O objetivo **atual** do projeto é migrar o
app de AngularJS 1.5 para Angular v21, adicionando dark mode e sistema de rascunhos,
mantendo paridade visual e funcional exata com o app existente.

**Repositório de referência:**
- Frontend: este repositório
- Backend (externo, imutável): API RealWorld em `https://conduit.productionready.io/api`

---

## Personas
Leitores e escritores de artigos (blog público). Personas não estão formalmente
documentadas além do produto Conduit (RealWorld spec).

---

## Escopo da missão atual

### Dentro do escopo
1. **Migração para Angular v21** (reescrita completa — §4.1 do guia)
2. **Dark mode** com alternador no header (camada de override, não redesign — §4.2)
3. **Sistema de rascunhos** via `localStorage` no editor (§4.3)
4. **Tarefas gerais:** issues/templates, DoR/DoD, Conventional Commits, bleeding
   branch, containerização (Docker + Compose), documentação OpenAPI do contrato
   consumido, testes de contrato + integração + E2E Playwright, mutação StrykerJS,
   LGTM (Loki/Grafana/Tempo/Mimir), script Python de validação.

### Fora do escopo
- Mudanças no backend ou contrato de API RealWorld.
- Alteração do design system visual (paleta, tipografia, classes Conduit, Ionicons).
- Qualquer mudança arquitetural não prevista no guia sem aprovação via `needs-decision`.

---

## Stack atual (baseline a preservar até migração)

| Camada | Tecnologia |
|---|---|
| Framework | AngularJS 1.x (`angular@^1.5.0-rc.2`) |
| Roteamento | UI-Router `^0.4.2` (hashbang, não html5Mode) |
| Build | Gulp 3 + Browserify + Babelify (preset es2015) + BrowserSync porta 4000 |
| Linguagem | ES6 transpilado para ES5; sem TypeScript |
| Auth | JWT em `localStorage['jwtToken']`; header `Authorization: Token <jwt>` |
| API base | `https://conduit.productionready.io/api` (em `app.constants.js`) |
| Markdown | `marked@^0.3.5` + `$sce.trustAsHtml` |
| Testes / CI / Docker | Nenhum |
| DevContainer | Node 8 (bloqueador — deve ser atualizado para Node LTS antes da migração) |

## Stack alvo (após migração)

| Camada | Tecnologia |
|---|---|
| Framework | Angular 21 (standalone components) |
| Build | Angular CLI (esbuild) |
| Roteamento | Angular Router (decisão hashbang vs history: ponto de permissão) |
| Estado | Signals vs RxJS/serviços (ponto de permissão) |
| SSR | Sim/Não via `@angular/ssr` (ponto de permissão — impacta Docker e LGTM) |
| Auth | JWT em `localStorage['jwtToken']`; header `Authorization: Token <jwt>` (preservado) |
| Markdown | Substituto de `marked` com `DomSanitizer` (ponto de permissão: escolha da lib) |
| Dark mode | CSS custom properties + classe `theme-dark` no `<body>`; `localStorage['conduit.theme']` |
| Rascunhos | `localStorage['conduit.draft.new']` / `localStorage['conduit.draft.<slug>']` |
| Testes | Contrato (OpenAPI), integração, E2E Playwright, mutação StrykerJS (meta 95%) |
| Container | Docker + Compose (Nginx ou Node SSR) + LGTM stack |
| Observabilidade | Loki, Grafana, Tempo, Mimir (mapeamento SPA vs SSR: ponto de permissão) |

---

## Módulos existentes (código legado)

| Módulo | Localização |
|---|---|
| article | `src/js/article/` |
| auth | `src/js/auth/` |
| components | `src/js/components/` |
| editor | `src/js/editor/` |
| home | `src/js/home/` |
| layout | `src/js/layout/` |
| profile | `src/js/profile/` |
| settings | `src/js/settings/` |
| services | `src/js/services/` (user, jwt, articles, comments, profile, tags) |
| config | `src/js/config/` (app.constants, auth.interceptor) |

---

## Riscos e pontos de atenção

- **Sem lockfile npm** → `npm install` pode resolver versões diferentes; pinar antes de
  qualquer trabalho no legado.
- **`strictDi: true` + `'ngInject'`** em todo o legado — irrelevante pós-migração, mas
  crítico se algo for tocado antes.
- **Esquema `Token`** (não `Bearer`) e envelopes de request/response: reproduzir exatamente.
- **`Articles.save()` muta o modelo** (`delete article.slug`) — relevante para rascunhos por slug.
- **Design system remoto** (`//demo.productionready.io/main.css`) — dark mode é camada de
  override, não edição do tema.
- `console.log` residual em `profile.controller.js` — não propagar para o app migrado.
- **Itens que NUNCA devem ser commitados:** `documentacao/`, `harness/`, `AGENTS.md`, `CLAUDE.md`
  (enforce via `.git/info/exclude`, não via `.gitignore` versionado).

---

## Pontos de permissão (decidir com aprovação humana antes de implementar)

1. Estratégia de estado: **Signals vs RxJS/serviços**
2. Roteamento: **hashbang vs history API (`RouterModule`)**
3. SSR: **`@angular/ssr` sim ou não** (impacta Docker, LGTM, script Python)
4. Biblioteca markdown substituta de `marked` (com sanitização via `DomSanitizer`)
5. Mapeamento LGTM para SPA: instrumentar servidor SSR vs interceptor de saída
6. Dark mode: vendorizar `main.css` vs camada de override (preferência: override)
7. Rascunhos: cobrir edição de artigo existente além de "novo" artigo
8. DoR/DoD: ajustes de preferência do time

---

## Definition of Done

Uma fase/issue está concluída quando:
- [ ] Critérios de aceite satisfeitos (sem mudanças não autorizadas de design/arquitetura)
- [ ] Testes de contrato verdes (sem regressão de API)
- [ ] E2E Playwright relevantes verdes
- [ ] Cobertura de mutação dentro da meta (quando aplicável)
- [ ] Commits em Conventional Commits referenciando `#<id>`
- [ ] Tema claro visualmente inalterado (quando aplicável)
- [ ] Prompts/skills registrados em `documentacao/` (§17 do guia)

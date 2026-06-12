# PLAN — 002-conventional-commits

- **Issue**: https://github.com/helio-alvs/angularjs-realworld-example-app/issues/2
- **Fase**: 1 — Fundação
- **Caminho operacional**: fast_path_A (Categoria A — tooling local, sem mudança de comportamento de produto)
- **Estado inicial**: PLANNED

---

## Objetivo

Configurar Conventional Commits no repositório com `commitlint` + `husky` (hook `commit-msg`) e criar a branch `bleeding` a partir de `master`.

---

## Pré-condições verificadas

- [x] Node 22 disponível (devcontainer configurado em `001`).
- [x] `gh` CLI disponível (v2.94.0 confirmado em STATE.md).
- [x] Issues habilitadas no repositório (Issue #1 e #2 criadas com sucesso).
- [x] `package.json` existe na raiz; sem `devDependencies` ainda.

---

## Decisões técnicas

| Decisão | Escolha | Motivo |
|---|---|---|
| Versão husky | v9 (latest stable) | API de hooks via `.husky/`; `prepare` script padrão da comunidade |
| Versão commitlint | v19 (latest stable) | Compatível com Node 22; suporte a ESM e CJS |
| Config commitlint | `@commitlint/config-conventional` | Padrão Angular/Conventional Commits — compatível com o histórico do projeto |
| Formato config | `.commitlintrc.js` (CJS) | `package.json` atual não tem `"type": "module"`; CJS é o default |
| Branch bleeding | `bleeding` a partir de `master` | Nome definido em STATE.md; serve como integration branch pré-main |

---

## Passos de implementação (para o builder)

### 1. Instalar dependências de desenvolvimento

```bash
npm install --save-dev husky @commitlint/cli @commitlint/config-conventional
```

### 2. Adicionar script `prepare` em `package.json`

Adicionar ao `package.json`:
```json
"scripts": {
  "prepare": "husky"
}
```

### 3. Inicializar husky e criar hook `commit-msg`

```bash
npx husky init
echo 'npx --no -- commitlint --edit "$1"' > .husky/commit-msg
chmod +x .husky/commit-msg
```

> `husky init` cria `.husky/` e sobrescreve `prepare` — confirmar que `prepare` ficou como `"husky"`.

### 4. Criar `.commitlintrc.js`

```js
// .commitlintrc.js
module.exports = {
  extends: ['@commitlint/config-conventional'],
};
```

### 5. Criar branch `bleeding`

```bash
git checkout -b bleeding
git push -u origin bleeding
git checkout master
```

### 6. Commit de tudo em `master` referenciando #2

```bash
git add package.json package-lock.json .husky/ .commitlintrc.js docs/agent/work/002-conventional-commits/
git commit -m "chore: add commitlint, husky commit-msg hook and bleeding branch (#2)"
```

---

## Critérios de aceite

- [ ] `devDependencies` em `package.json` contém `husky`, `@commitlint/cli`, `@commitlint/config-conventional`.
- [ ] `.husky/commit-msg` existe e contém a chamada ao `commitlint`.
- [ ] `.commitlintrc.js` existe na raiz com `extends: ['@commitlint/config-conventional']`.
- [ ] `npm run prepare` executa sem erro.
- [ ] Commit com mensagem inválida (`bad commit`) é rejeitado pelo hook.
- [ ] Commit com mensagem válida (`chore: teste`) é aceito pelo hook.
- [ ] Branch `bleeding` existe no remoto apontando para o mesmo SHA de `master`.
- [ ] `docs/agent/work/002-conventional-commits/PLAN.md` commitado referenciando `#2`.
- [ ] `docs/agent/STATE.md` atualizado para slice 002 em READY.

---

## Gates determinísticos (fast_path_A)

| Gate | Comando | Critério de aprovação |
|---|---|---|
| `npm install` limpo | `npm install` | Saída sem erros; `package-lock.json` gerado/atualizado |
| Hook funcional (negativo) | `echo "bad commit" \| npx commitlint` | Retorna exit code ≠ 0 com mensagem de erro |
| Hook funcional (positivo) | `echo "chore: test" \| npx commitlint` | Retorna exit code 0 |
| Branch remota | `git ls-remote --heads origin bleeding` | SHA listado |

---

## Riscos residuais

- `husky init` pode sobrescrever um script `prepare` existente — não há `prepare` no `package.json` atual, portanto risco baixo.
- Se o runner CI/CD não executar `npm install` com `--ignore-scripts`, o hook não será instalado em pipelines — aceitável nesta fase (CI ainda não configurado).

---

## Model Profile (fast_path_A)

- **Tier**: standard
- **Effort**: low
- **Motivo**: instalação de ferramentas conhecidas, sem lógica de produto, sem fronteira de segurança.

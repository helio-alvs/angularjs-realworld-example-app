# Prompt 01 — Planejar & Triar (papel: planner)

> **Quando usar:** ao iniciar uma nova unidade de trabalho (uma "slice": uma
> mudança pequena, coerente e revisável). Este prompt **classifica o risco**,
> escolhe o **caminho operacional**, e produz o **plano** com critérios de
> aceite, gates e perfil de modelo — antes de qualquer código.
>
> **Como usar:** cole o bloco abaixo e, no fim, descreva a intenção em uma
> linha. Ex.: *"Adicionar validação de JWT ao endpoint /api/v1/files."*

=====================================================================

Você é o **planner**. Responda em **PT-BR**; mantenha identificadores técnicos
em inglês. Você ainda **não vai escrever código de produto** neste turno — seu
entregável é o plano e a proposta de pré-flight.

## Passo 0 — Pré-requisitos e leitura

1. Se **`docs/agent/STATE.md` não existe**, pare e diga: *"Rode o Prompt 00 —
   Onboarding antes de planejar."* (sem contexto não há plano seguro).
2. Leia, nesta ordem: `docs/agent/OPERATING-GUIDE.md`,
   `docs/agent/STATE.md`, `docs/agent/context/CHARTER.md`,
   `docs/agent/context/ROADMAP.md`, `docs/agent/context/SOURCES.md`.
3. Confirme a **intenção** da slice em uma frase. Se ela for ambígua ou não
   couber na fase atual do `ROADMAP.md`, **pare e pergunte** ao usuário.

## Passo 1 — Classificar o RISCO (A/B/C/D)

Classifique de **D para A** (o primeiro match vence). Cite o gatilho específico:

- **D** — cripto/KDF/assinatura/RNG de segurança, dados regulados em runtime,
  protocolo de segurança próprio.
- **C** — auth/authz, sessão/token/JWT/OAuth, secrets, path traversal, upload/
  download com input, shell com input, SSRF, CORS/CSP, webhook signature,
  rate-limit público. (Remover um check de segurança é C.)
- **B** — muda estado/comportamento visível sem cruzar auth/secrets.
- **A** — não cruza fronteira de confiança.

> Risco se mede por **fronteira de confiança**, não por nº de linhas. Um fix de
> 3 linhas num validador de JWT é **C**. Um refactor de 500 linhas de CSS é
> **A**. Na dúvida entre duas categorias, **escolha a maior** e registre a
> ambiguidade.

## Passo 2 — Escolher o CAMINHO operacional

Use os **sinais de escopo** (binários) para confirmar o caminho:
`touches_executable_code`, `touches_build_deploy`, `touches_secrets_config`,
`touches_auth`, `touches_database`, `touches_external_integration`,
`touches_money_movement`, `touches_crypto_primitive`, `touches_regulated_data`.

| Sinais / risco | Caminho |
|---|---|
| Tudo NÃO, risco A, só docs/typo/formatação | `trivial_path` |
| Tudo NÃO, risco A, qualquer outra coisa | `fast_path_A` |
| `touches_executable_code` ou `touches_database` ou `touches_build_deploy` | `standard_path_B` (mínimo) |
| `touches_secrets_config` ou `touches_auth` ou `touches_external_integration` | `strict_path_C_D` (C) |
| `touches_money_movement` ou `touches_crypto_primitive` ou `touches_regulated_data` | `strict_path_C_D` (D) |

**Desempate:** o caminho mais alto vence. **Na dúvida, escale.** O custo de
rodar `strict_path_C_D` numa slice B é um turno de avaliador a mais; o custo de
rodar `trivial_path` numa slice C é uma brecha de segurança.

### Atalho do `trivial_path`
Se for genuinamente trivial (docs/typo/comentário/formatação, sem tocar código
executável nem config sensível), **não** produza `PLAN.md`. Registre só os
**3 itens** em `docs/agent/work/<slug>/PROGRESS.md`: descrição em uma linha;
`risk_category: A` explícito; atestação de que não toca código/config sensível.
Depois pule direto ao Prompt 02 (ou faça a edição e encerre com o `### Handoff`).

## Passo 3 — Pre-Flight Checklist (proponha, não pergunte em branco)

Para `fast_path_A` cubra os itens 1, 2, 6, 7 (versão enxuta). Para
`standard_path_B` e `strict_path_C_D`, cubra **todos**. Você **traz a proposta
pronta** — o usuário confirma ou corrige:

1. **Identificação da slice** — fase do `ROADMAP.md`, slug, e a intenção
   verbatim. **Bind dos context inputs:** case a intenção com os SKILL.md
   registrados (pelo `description`) e, para slices de frontend/design-core, com
   o DESIGN. Liste os caminhos que governam a slice em `context_sources`.
   Ausência é OK: se nada se aplica, deixe vazio e siga as convenções do código.
2. **Model Profile** — tier + effort por papel (planner, generator, evaluator,
   reviewer; + security_reviewer se C/D). Justifique em uma frase por papel.
3. **Evaluator cross-family** — a slice é fact-sensitive (regulação, recall de
   API, fatos públicos)? Se sim, o evaluator deve ser de **família diferente**
   do generator. Marque `cross_family_evaluator: true`.
4. **Orçamento** — estimativa de custo por papel e `budget_max_usd` da slice
   (heurística: `turns_expected * custo_médio_por_turno * 1.5`). Defina gatilhos
   de ALERTA/STOP de custo se o projeto usar isso.
5. **Transporte** — cloud, local ou híbrido (relevante p/ dados sensíveis ou
   ambiente air-gapped).
6. **Classificação de risco** — categoria (A/B/C/D) + gatilho citado. Se C/D,
   liste também os **deterministic gates** que vão rodar (>= 4 válidos),
   confirme o `security_reviewer` e o `human_review: required`.
7. **Condições de parada** — o que faria você **parar e perguntar** em vez de
   seguir sozinho (orçamento estourado, falha de sensor recorrente, ambiguidade
   nova, contrato de API diferente do esperado, red flag de segurança).
8. **Política de cobertura** — só se a slice mexe em teste: cite/defina o mínimo
   de cobertura por módulo afetado (+10pp para módulos sensíveis: auth, secrets,
   pagamentos, compliance). Marque "N/A" com justificativa caso contrário.

Apresente isso como uma **proposta** e peça: *"Confirme 'ok' para eu adotar
esta proposta, ou diga o que mudar."* **Não escreva código** enquanto o Model
Profile não estiver definido. Se o usuário responder "ok"/"siga", adote a sua
própria proposta.

> **Decisão bloqueante ≠ risco em aberto.** Se o plano depende de uma decisão
> que precisa ser tomada ANTES de implementar — escolha de **stack, framework ou
> banco**, ou qualquer decisão arquitetural digna de ADR — ela **não** é um
> `open_risk` que segue para o builder: é uma **`pending_decision`** que trava a
> implementação. Apresente as **opções autorizadas** (do CHARTER/PRD) + sua
> **recomendação** e **pare para o usuário escolher**. Nunca invente a stack
> para "destravar" e nunca declare uma decisão dessas como auto-adotada.

## Passo 4 — Escrever o `PLAN.md`

Crie `docs/agent/work/<NNN-slug>/PLAN.md` (numere a slice; slug curto em
kebab-case). Use este template:

```markdown
# PLAN — <slug>

## Status
State: PLANNED

## Objetivo
<uma frase verificável do resultado esperado>

## Escopo
- Incluído:
- Fora de escopo:
- files_owned:           # arquivos/dirs que esta slice pode tocar
  - <path>

## Origem e fase
- Fase (ROADMAP): <N — nome>
- Documento/bloco de origem: <PRD/ADR/SKILL/entrevista>
- context_sources:        # context inputs que GOVERNAM esta slice (ou vazio)
  - skills: []            # caminhos de SKILL.md casados por description
  - design: null          # caminho do DESIGN se frontend/design-core
- affects_design_system_core: false

## Dados, segurança e compliance
- Dados envolvidos / sensibilidade:
- Permissões / minimização / mascaramento:
- Auditoria / log (sem PII, sem secrets):
- Revisão humana:

## API, entidades e integrações
- Endpoints/contratos / entidades/tabelas / integrações externas / jobs/eventos:

## Critérios de aceite
- [ ] <critério verificável 1>
- [ ] <critério verificável 2>
- [ ] Documentação e riscos atualizados.

## Operational path & risk
- risk_category: <A|B|C|D>   # gatilho: <cite o gatilho>
- operational_path: <trivial|fast_A|standard_B|strict_C_D>

## Model Profile
```yaml
risk_category: <A|B|C|D>
planner:   { tier: <light|standard|deep>, effort: <...> }
generator: { tier: <...>, effort: <...> }
evaluator: { tier: <...>, effort: <...> }   # família diferente se fact-sensitive
reviewer:  { tier: <...>, effort: <...> }
# Obrigatório em C/D:
# security_reviewer: { tier: deep, effort: xhigh }
# deterministic_gates: [sast, dep_scan, secret_scan, negative_tests, config_review, audit_log_check]
# human_review: required           # D adiciona: especialista (security/cripto/DPO)
cross_family_evaluator: <true|false>
budget_max_usd: <valor>            # (turns * custo_médio) * 1.5
rationale: |
  <categoria + gatilho; por que este tier/effort por papel; trade-offs>
```

## Deterministic gates (a rodar antes de READY)
- <build | unit_tests | lint | typecheck | sast | dep_scan | secret_scan | negative_tests | ...>
  (>= 4 VÁLIDOS para C/D — "LLM revisou" / "parece correto" NÃO contam)

## Condições de parada
- <lista>

## Riscos e pendências
- Risco:
- Pendência:
```

Crie também o esqueleto de `docs/agent/work/<slug>/PROGRESS.md`:

```markdown
# PROGRESS — <slug>

State: PLANNED

## Timeline
| Data/hora | Papel | State | Evidência |
|---|---|---|---|

## Decisões
-

## Arquivos alterados
-

## Comandos e resultados
| Comando | Resultado |
|---|---|

## Riscos residuais / pendências
-
```

## Passo 5 — Portão de confirmação, estado e handoff

1. Atualize `docs/agent/STATE.md` (`Active work unit: docs/agent/work/<slug>`,
   `State: PLANNED`) e registre a primeira linha na `Timeline` do `PROGRESS.md`.
2. **Regra dura para o `next_role`:** use `builder` **somente** se (a) o usuário
   já aprovou o plano (disse "ok"/"siga") **e** (b) não há `pending_decision` em
   aberto. Caso contrário, `next_role: planner` e a pergunta vai em
   `next_action`. **Plano escrito não é plano aprovado.**
3. Encerre a resposta com **duas coisas, nesta ordem**: o bloco `### Handoff` e,
   logo abaixo, **uma linha explícita para o usuário** — ele não deve ter que
   adivinhar entre "mandar continuar" e "rodar o Prompt 02". Só a resposta + o
   bloco + a linha; nada de raciocínio interno.

```
### Handoff
- state: PLANNED
- this_turn: planner — plano proposto para "<slug>"
- evidence: docs/agent/work/<slug>/PLAN.md
- open_risks: <riscos que SEGUEM para a implementação — não decisões que a travam>
- pending_decision: <decisão que o usuário precisa tomar antes de implementar | none>
- next_role: <planner se há pending_decision ou falta aprovação | builder se aprovado>
- next_action: <a pergunta/decisão ao usuário | "implementar a slice seguindo PLAN.md">
- suggested_model: <tier + effort> — <motivo>
```

**Próximo passo para você** (escreva SEMPRE esta linha, em PT-BR, após o bloco):
- Se `next_role: planner` → *"Antes de implementar preciso da sua decisão sobre
  `<X>`: `<opções autorizadas + minha recomendação>`. Responda aqui."*
- Se `next_role: builder` → *"Revise/edite o `PLAN.md`. Quando aprovar, rode o
  Prompt 02 — Implementar."*

=====================================================================

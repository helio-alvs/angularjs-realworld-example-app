# Tarefa: executar UM turno e emitir o handoff

## Contrato de saída (hard requirement)

A resposta visível contém **exatamente duas partes**, nesta ordem:

1. O entregável do papel atual (`current_role`), em PT-BR — sem raciocínio interno, meta-comentário ou preâmbulo.
2. O bloco `next-turn-hint` **renderizado literalmente** como fenced block ao final.

Planejar o bloco ≠ emiti-lo. Turno sem o bloco cercado renderizado = contrato violado.

## Formato EXATO do bloco

```next-turn-hint
current_role:            <planner|generator|evaluator|reviewer|security_reviewer|doc_gardener>
current_turn:            <m/n>
next_role:               <role|done>
next_tier:               <light|standard|deep>
next_model:              <provider/model>
next_model_alternatives: <provider/model>, <provider/model>   (ou n/a)
next_effort:             <minimal|low|medium|high|xhigh>
rationale:               <uma frase>
estimated_cost:          <$0.00>
Swap commands:
  Claude Code:    /model <model-id>
  Codex CLI:      exit; codex exec --model <model-id> "<continue>"
  Antigravity:    select <model-id> in the chat header
  Cursor:         Cmd-. / Ctrl-. e selecionar <model-id>
  Host runtime:   auto (runtime will apply)
```

Regras do bloco:

- Preencher todos os campos; nunca omitir linhas nem alterar chaves.
- Se `next_role: done`, preencher os demais campos `next_*` com `n/a`.

## Disciplina de issues

- Tracking via GitHub Issues, com templates do repositório; DoR/DoD documentados por repositório.
- Criar a issue **antes** de iniciar o trabalho; toda alteração no repositório referencia sua issue (`#<id>`).

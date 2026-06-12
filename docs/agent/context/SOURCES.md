# SOURCES — Fontes registradas do projeto

| Caminho | Tipo | Autoridade | Status | Escopo / gatilho |
|---|---|---|---|---|
| `src/documentacao/GUIA-DE-REFATORACAO.md` | prd + roadmap (híbrido) | authoritative | in-repo (não versionado — §13 do guia proíbe commit) | Fonte de verdade: identidade, invariantes, mudanças autorizadas (§4/§5), DoR/DoD, riscos, roadmap de 6 fases, protocolo de permissão (§14) |

---

## Notas

- **ADR:** Não há ADRs separados. Decisões arquiteturais e seus racionais estão
  inline no `GUIA-DE-REFATORACAO.md` (especialmente §2, §4, §14).
- **DESIGN.md:** Não existe arquivo separado. O design system é 100% remoto
  (`//demo.productionready.io/main.css` + Ionicons + Google Fonts); as restrições
  visuais estão descritas em §3.1 do guia. Caso o time deseje formalizar, pode-se
  esboçar um `DESIGN.md` a partir do §3.1.
- **SKILL.md:** Não existe. Caso haja playbooks de execução por tipo de trabalho,
  registrar aqui com o campo `description` como gatilho de seleção.
- **Roadmap:** Embutido no §15 do guia; expandido em `docs/agent/context/ROADMAP.md`.
- **OpenAPI:** A ser criado na Fase 2 (documentação do contrato consumido).

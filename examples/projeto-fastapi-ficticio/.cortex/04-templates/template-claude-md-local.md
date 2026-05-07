# CLAUDE.md — {{PROJECT_NAME}}

## Projeto

Nome: {{PROJECT_NAME}}

Objetivo: {{PROJECT_OBJECTIVE}}

---

## Arquivo central de orientação

Antes de qualquer trabalho estruturado neste projeto, leia:

```text
./.cortex/Claude.md
```

`.cortex/` é a camada de disciplina operacional do harness, contém todas as regras, fluxos, agentes e skills que governam como o projeto funciona. O agente consulta .cortex/Claude.md antes de qualquer ação estruturada, garantindo consistência e evitando deriva entre sessões.

---

## Comandos operacionais

Quando você disser "Use o fluxo X", "Acione o agente Y" ou equivalente, busque a interpretação em `./.cortex/Claude.md`, não neste arquivo.

---

## Papel deste arquivo

Este `CLAUDE.md` local serve apenas como ponto de conexão entre este projeto e o arquivo central de orientação.

As regras de trabalho, critérios, fluxos, padrões e demais instruções devem ser obtidos a partir do arquivo central e dos documentos que ele indicar.

---

## Registros locais

- `docs/` — documentação do projeto
- `memory/` — contexto persistente entre sessões
- Não copie conteúdo de `.cortex/` para outro lugar

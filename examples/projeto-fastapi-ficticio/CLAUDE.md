# CLAUDE.md

Este arquivo conecta este projeto a uma base externa de orientação operacional para uso com Claude.

Ele deve ser mantido simples e não deve duplicar regras, fluxos, padrões ou instruções que pertencem à base externa.

---

## Projeto

Nome: livraria-comunitaria

Objetivo: API de apoio à biblioteca comunitária do bairro — cadastrar livros, registrar empréstimos entre vizinhos, marcar devoluções e listar empréstimos em aberto.

---

## Arquivo central de orientação

Antes de executar qualquer trabalho estruturado neste projeto, consulte o arquivo central abaixo:

```text
./.cortex/Claude.md
```

Esse arquivo é a entrada principal para entender como o trabalho deve ser conduzido.

### Como preencher `{{CENTRAL_CLAUDE_MD_PATH}}`

Use **path relativo a partir da raiz do projeto**, nunca path absoluto.

A forma recomendada é o **Modelo B** (governança copiada para `.cortex/` dentro do projeto):

```text
./.cortex/Claude.md
```

Estrutura esperada no projeto após instalação Modelo B:

```text
livraria-comunitaria/
├── .cortex/                  ← cópia da governança (01-regras, 02-agentes, ...)
│   └── Claude.md             ← arquivo central referenciado por este template
├── CLAUDE.md                 ← este arquivo, com placeholder preenchido
├── docs/
└── memory/
```

Path relativo é obrigatório porque o projeto pode ser clonado em diferentes máquinas, versionado em Git, ou compartilhado com colaboradores.

---

## Obrigatoriedade de consulta prévia

Antes de responder a qualquer solicitação operacional, trabalho estruturado ou comando de fluxo neste projeto, o agente deve consultar obrigatoriamente o arquivo central de orientação indicado acima.

Este `CLAUDE.md` local não contém as regras operacionais do sistema. Ele apenas conecta este projeto ao arquivo central.

Se o usuário solicitar algo como "Use o fluxo...", "Acione o fluxo..." ou equivalente, a interpretação e execução desse comando devem ser buscadas no arquivo central, não neste arquivo local.

O agente não deve responder a comandos operacionais apenas com base neste arquivo local.

---

## Papel deste arquivo

Este `CLAUDE.md` local serve apenas como ponto de conexão entre este projeto e o arquivo central de orientação.

As regras de trabalho, critérios, fluxos, padrões e demais instruções devem ser obtidos a partir do arquivo central e dos documentos que ele indicar.

---

## Contexto local

Registre neste projeto apenas informações específicas deste projeto.

Não copie para este repositório conteúdos que pertencem à base externa de orientação, salvo quando o arquivo central orientar explicitamente.

---

## Limites

Não altere a base externa de orientação a partir deste projeto.

Não modifique arquivos fora do escopo deste projeto, salvo quando solicitado explicitamente pelo usuário.

Caso falte orientação para executar uma tarefa, registre a lacuna e solicite confirmação antes de criar novas regras ou estruturas.

---

## Uso esperado

Após este arquivo estar configurado, os comandos ao Claude podem ser simples, mas devem considerar se o contexto local já foi carregado.

### Em sessão nova, conversa limpa ou contexto não carregado

```text
Leia o CLAUDE.md local e use o fluxo Init Project para iniciar este projeto.
```

```text
Leia o CLAUDE.md local e use o fluxo Handoff para encerrar esta sessão.
```

### Com contexto local já carregado

```text
Use o fluxo Init Project para iniciar este projeto.
```

```text
Use o fluxo Handoff para encerrar esta sessão.
```

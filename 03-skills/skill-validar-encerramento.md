# Skill: Validar encerramento

## Objetivo
Verificar, ao final de um workflow que produz documentação, que o material entregue não contém pendências silenciosas — placeholders não substituídos, referências a arquivos que não existem, fechamentos parciais e similares.

Esta skill assume que o harness é responsável ativo pela detecção de pendências documentais. Não delega 100% à atenção do operador.

Relacionado a:
- [Claude](../Claude.md)
- [00-regras-gerais](../01-regras/00-regras-gerais.md)
- [03-checklist-de-qualidade](../01-regras/03-checklist-de-qualidade.md)
- [04-estrutura-documental-de-projetos](../01-regras/04-estrutura-documental-de-projetos.md)
- [agente-inicializacao-projeto](../02-agentes/agente-inicializacao-projeto.md)
- [agente-arquiteto](../02-agentes/agente-arquiteto.md)
- [agente-planejador](../02-agentes/agente-planejador.md)
- [agente-analista](../02-agentes/agente-analista.md)

---

## Quando usar
Usar esta skill ao final de qualquer workflow que produz ou edita documentação no projeto do adopter, em particular:

- Init Project (novo-projeto)
- Setup Project
- Create Feature
- Refatoração
- Review
- Learn Simple
- Session Handoff

A invocação típica é a última etapa do workflow, antes de declarar o trabalho como concluído.

---

## O que esta skill verifica

### 1. Placeholders pendentes
Procurar nos arquivos produzidos pelo workflow os seguintes padrões:

- `{{NOME_QUALQUER}}` — placeholder do tipo template (qualquer texto entre chaves duplas, em maiúsculas e underscores).
- `<!-- TODO: ... -->` — comentário HTML com prefixo TODO.
- `<!-- TODO -->` — variação curta sem descrição.

Cada match é uma pendência. Tratar como placeholder não substituído.

### 2. Referências a arquivos inexistentes
Identificar nos arquivos produzidos toda menção a outro arquivo, nos seguintes formatos:

- `Ver: <path>` ou `Ver: \`<path>\``
- `ver: <path>` (variações de capitalização)
- Links markdown `[texto](path)` onde `path` é caminho local (não `http://` nem `https://`).
- Wikilinks no formato Obsidian (`[ [arquivo] ]` sem os espaços) — no contexto do adopter raramente existem, mas se aparecerem, validar.

Para cada referência, verificar se o arquivo existe no caminho indicado, relativo à raiz do projeto do adopter. Se não existir, é uma referência pendurada.

### 3. EOF/newline final
Cada arquivo produzido pelo workflow deve terminar em quebra de linha. Falta de newline final conta como fechamento parcial.

### 4. Seções vazias com cabeçalho
Cabeçalhos markdown (`#`, `##`, `###`) sem conteúdo abaixo (apenas linhas em branco até o próximo cabeçalho) são seções vazias. Cada uma conta como pendência.

---

## Passos

### 1. Receber a lista de arquivos produzidos
O workflow passa para a skill a lista dos arquivos criados ou editados durante a execução. A skill opera sobre essa lista, não sobre o projeto inteiro.

### 2. Aplicar os 4 checks
Rodar as 4 verificações descritas acima em cada arquivo da lista.

### 3. Consolidar achados
Agrupar achados por tipo:

```text
Placeholders pendentes:
  - CLAUDE.md:1 → {{PROJECT_NAME}}
  - CLAUDE.md:5 → {{PROJECT_NAME}}
  - CLAUDE.md:7 → {{PROJECT_OBJECTIVE}}

Referências penduradas:
  - docs/features/ted-response.md:98 → Ver: memory/learnings_ted_response.md (arquivo não existe)

EOF/newline ausente:
  - docs/context/current-status.md

Seções vazias:
  - docs/features/ted-response.md:### Aprendizados Registrados (vazia)
```

Se nenhum achado, reportar: "Encerramento limpo. Nada a corrigir."

### 4. Apresentar ao usuário
Mostrar o relatório consolidado. Para cada categoria, perguntar:

- **Placeholders:** "Substituir agora ou seguir com pendências registradas?"
- **Referências penduradas:** para cada uma, oferecer 3 opções:
  - (a) criar o arquivo referenciado agora;
  - (b) remover a referência;
  - (c) substituir por outra fonte que exista.
- **EOF:** corrigir automaticamente é razoável (decisão do agente que invocou).
- **Seções vazias:** "Preencher agora, remover seção, ou seguir como pendência?"

### 5. Registrar decisões e pendências
Se o usuário decidir seguir com alguma pendência, registrar em `docs/context/current-status.md` como pendência aberta, com data e descrição.

Se o usuário corrigir tudo, apenas reportar "validação de fechamento concluída sem pendências".

---

## Saída esperada

Ao final da skill deve existir:

- relatório consolidado dos 4 checks aplicados aos arquivos produzidos pelo workflow;
- decisão do usuário registrada para cada achado (corrigir ou pendência);
- pendências aceitas registradas em `docs/context/current-status.md`;
- workflow pode declarar encerramento.

---

## Princípio de fronteira

Esta skill **não revisa qualidade de conteúdo**. Para isso, usar [skill-revisar-documento](skill-revisar-documento.md).

Esta skill verifica apenas fechamento mecânico:
- placeholder substituído?
- referência resolve?
- arquivo termina bem?
- seção tem conteúdo?

Esta skill **não opera sobre o projeto inteiro**, apenas sobre os arquivos produzidos ou editados pelo workflow que a invocou. Operação ampla é fora do escopo.

Esta skill **não valida URLs HTTP**, apenas referências internas a arquivos locais.

---

## Padrão de marcação de placeholders nos templates

Para que esta skill funcione bem, os templates em `04-templates/` adotam padrão visual explícito:

```markdown
# CLAUDE.md — {{PROJECT_NAME}} <!-- TODO: substituir pelo nome do projeto -->
```

O par `{{...}}` + `<!-- TODO: ... -->` cumpre três funções:
- visibilidade ao operador (TODO aparece em qualquer renderizador);
- pesquisabilidade por regex (a skill encontra ambos);
- documentação inline do que substituir.

Templates novos devem seguir esse padrão. Templates existentes que ganhem placeholders também.

---

## Regras aplicáveis
- [00-regras-gerais](../01-regras/00-regras-gerais.md)
- [03-checklist-de-qualidade](../01-regras/03-checklist-de-qualidade.md)
- [04-estrutura-documental-de-projetos](../01-regras/04-estrutura-documental-de-projetos.md)

---

## Agentes que mais usam esta skill
Todos os agentes que conduzem workflows que produzem documentação:

- [agente-inicializacao-projeto](../02-agentes/agente-inicializacao-projeto.md) (Init Project, Setup Project)
- [agente-arquiteto](../02-agentes/agente-arquiteto.md) (Create Feature, Refatoração)
- [agente-analista](../02-agentes/agente-analista.md) (Review, Learn Simple)
- [agente-planejador](../02-agentes/agente-planejador.md) (Session Handoff)

---

## Sinais de uso correto
- placeholders não passam silenciosamente para o trabalho do adopter;
- referências penduradas são detectadas e tratadas antes do encerramento;
- pendências aceitas ficam visíveis em `docs/context/current-status.md`;
- adopter não descobre fricção tipo "esqueci de substituir {{...}}" depois.

---

## Sinais de uso ruim
- skill rodada sem o usuário ver o relatório;
- correções aplicadas automaticamente sem confirmação;
- skill expande para revisar qualidade de conteúdo (fora do escopo);
- skill opera no projeto inteiro em vez dos arquivos do workflow;
- pendências registradas em arquivos paralelos em vez de `current-status.md`.

---

## Origem
Esta skill foi criada em 2026-05-12 para implementar as Decisões 03 (placeholders visíveis em templates) e 04 (workflows sem referências penduradas), registradas em `11-evolucao-do-sistema/02-decisoes/` a partir de fricções identificadas no teste-adopter-1.

---

## Regra final
O papel desta skill é simples:
**garantir que workflows que produzem documentação não fechem com pendências silenciosas — placeholders, referências penduradas, fechamentos parciais.**

# Skill: Validar encerramento

## Objetivo
Verificar, ao final de um workflow que produz documentação, que o material entregue não contém pendências silenciosas — placeholders não substituídos, referências a arquivos que não existem, fechamentos parciais, `.gitignore` mínimo ausente, wikilinks fantasma — antes de declarar o workflow concluído.

Esta skill é **OBRIGATÓRIA** nos 7 workflows listados em "Quando usar". Pular esta skill é considerado **quebra de protocolo do harness**, equivalente a executar sem mostrar plano. Ver `Claude.md` central, seção "Skills obrigatórias por workflow".

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
Usar esta skill ao final de qualquer workflow que produz ou edita documentação no projeto do adopter. Obrigatória em todos os 8 workflows:

- Init Project (novo-projeto)
- Setup Project
- Create Feature
- Refatoração
- Review
- Learn Simple
- Session Handoff
- Cortex Spec Flow (com matriz de checks adaptada por modo — ver seção abaixo)

A invocação é a última etapa do workflow. O workflow **não pode ser declarado concluído** sem o relatório da skill ter sido exibido — mesmo que zero problemas tenham sido encontrados.

**Exceção parcial — modo Exploração do Cortex Spec Flow:** a skill ainda roda (hard-block sobre execução continua), mas o relatório é silencioso quando todos os checks aplicáveis estão limpos. Preserva natureza do modo. Check 7 (destino declarado) é sempre obrigatório, mesmo em Exploração. **Mesmo no modo silencioso, a skill sempre exibe ao menos a confirmação do Check 7** (1 linha: destino declarado) — preserva o princípio "o adopter vê que a skill rodou" (Decisão 05) sem trazer de volta o ritual completo.

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

**Exclusão explícita:** wikilinks que estão na lista negra carregada pelo Check 6 (ver `03-skills/wikilinks-fantasma.txt`) **não** são reportados aqui. Esses são tratados pelo Check 6 com sugestões específicas de substituição, evitando alertas duplicados sobre a mesma linha. Demais wikilinks (que não estão na lista negra) seguem validados normalmente por este check.

### 3. EOF/newline final
Cada arquivo produzido pelo workflow deve terminar em quebra de linha. Falta de newline final conta como fechamento parcial.

### 4. Seções vazias com cabeçalho
Cabeçalhos markdown (`#`, `##`, `###`) sem conteúdo abaixo (apenas linhas em branco até o próximo cabeçalho) são seções vazias. Cada uma conta como pendência.

### 5. `.gitignore` mínimo (somente Init Project e Setup Project)

Aplicar este check **apenas** na conclusão de Init Project e Setup Project. Não rodar nos outros 5 workflows.

Procedimento:

1. Ler `.gitignore` na raiz do projeto do adopter. Se não existir, considerar conteúdo vazio.
2. Verificar presença das 10 entradas mínimas da regra P11.1 (ver `01-regras/04-estrutura-documental-de-projetos.md`, seção "Proteção mínima com `.gitignore`"):

   **Fora do Git por default (sem decisão a tomar):**
   ```
   .claude/
   tasks/
   node_modules/
   logs/
   AGENTS.md
   *.pyc
   __pycache__/
   .DS_Store
   ```

   **Fora do Git por default — mas com permissão de versionar caso a caso:**
   ```
   docs/
   memory/
   ```

3. Para cada entrada faltante, listar no relatório **agrupada por grupo de origem**:
   - **Grupo "sem decisão a tomar"** (8 entradas) — listar como bloco "Fora do Git por default":
     ```
     .claude/, tasks/, node_modules/, logs/, AGENTS.md, *.pyc, __pycache__/, .DS_Store
     ```
   - **Grupo "com permissão de versionar caso a caso"** (2 entradas) — listar como bloco separado "Fora do Git por default, mas com permissão de versionar arquivo específico via regra negativa explícita":
     ```
     docs/, memory/
     ```
   A distinção visual evita que o adopter assuma que `docs/` e `memory/` viraram bloqueados — eles continuam permitindo exceções via `!docs/foo.md` ou `!memory/bar.md`.
4. Perguntar ao adopter: `Adicionar as entradas faltantes ao .gitignore? [s/N]`
5. Se "s", anexar as entradas faltantes ao arquivo (idempotente — não duplica linha existente, preserva conteúdo original, mantém comentários). **Ao anexar entradas do grupo "com permissão"** (`docs/` e/ou `memory/`), registrar nota curta em `docs/context/current-status.md` lembrando que arquivos específicos dessas pastas podem ser versionados via regra negativa explícita (ex: `!docs/business/domain-overview.md`).
6. Se "n", registrar como pendência aceita em `docs/context/current-status.md`.

Este check **não é hard-block** sobre conteúdo do `.gitignore` — adopter pode recusar e seguir. O hard-block é apenas a execução da skill em si.

### 6. Wikilinks fantasma

Procedimento:

1. Carregar lista negra de `03-skills/wikilinks-fantasma.txt` (formato: uma ref por linha, separador ` → ` indicando sugestão de substituição).
2. Varrer os arquivos produzidos pelo workflow procurando ocorrências dos wikilinks listados.
3. Para cada ocorrência, mostrar no relatório com a sugestão de substituição.
4. Perguntar ao adopter: `Substituir automaticamente? [s/N]`
5. Se "s", aplicar substituições idempotentes mantendo a formatação ao redor.
6. Se "n", registrar como pendência aceita em `docs/context/current-status.md`.

Este check opera **apenas sobre arquivos produzidos pelo workflow** — não varre toda a governança do `.cortex/`. Arquivos da governança no vault do autor podem conter refs que resolvem ali e viram fantasma só no OSS público; o script de build é responsável pela limpeza dessas, não esta skill.

### 7. Fluxo encerrado (somente Cortex Spec Flow)

Aplicar este check **apenas** na conclusão de [cortex-spec-flow](../08-orquestracao/tipos-de-trabalho/cortex-spec-flow.md). Não rodar nos outros workflows.

Procedimento:

1. Localizar `docs/specs/<nome>/assinatura.md`. Se não existir, é uma "spec órfã" — alertar.
2. Verificar que `assinatura.md` contém destino declarado explicitamente, com 1 de 4 opções válidas:
   - **Create Feature** — registrar link para a spec consumível (`docs/specs/<nome>/visao.md` ou equivalente).
   - **Pausa** — registrar motivo e gatilho de retomada.
   - **Descarte** — registrar motivo (validação negativa, mudança de prioridade, etc.).
   - **Discovery adicional** — registrar próxima ação concreta.
3. Em modo Governado, validar também o checklist completo:
   - os 7 artefatos das 8 etapas do modo Governado existem: `visao.md`, `usuarios.md`, `jornadas.md`, `casos-de-uso.md`, `regras-negocio.md`, `criterios.md`, `tasks.md`;
   - `decisoes.md` é **condicional** — obrigatório apenas se a spec registrou decisões (IDs `DEC-XX`); ausência é válida quando nenhuma decisão foi registrada;
   - cada arquivo presente contém pelo menos uma seção preenchida.
4. Em modo Exploração, validar destino simples (1 linha em `assinatura.md` é suficiente).
5. Em modos Guiado/Conduzido, validar coerência mínima entre modo e destino (ex: destino "Create Feature" exige `visao.md` + algum bloco de casos ou regras).
6. Para cada lacuna detectada, listar no relatório com sugestão de resolução.
7. Perguntar ao adopter: `Resolver lacunas agora ou seguir com pendências registradas?`

Este check **não é hard-block** sobre o conteúdo de `assinatura.md` em si — adopter pode aceitar destino simples e seguir. O hard-block é apenas sobre a **presença** do destino declarado: sem destino algum, workflow não pode fechar.

A skill auxiliar [skill-cortex-spec-flow](skill-cortex-spec-flow.md) apoia este check enriquecendo o relatório com sugestões específicas por modo, mas o Check 7 fica nesta skill (mantém hard-block na obrigatoriedade — Decisão 05).

---

## Matriz de checks por modo do Cortex Spec Flow

Quando o workflow invocador for [cortex-spec-flow](../08-orquestracao/tipos-de-trabalho/cortex-spec-flow.md), a skill aplica os checks com adaptação por modo:

| Check | Exploração | Guiado | Conduzido | Governado |
|---|---|---|---|---|
| 1. Placeholders | silencioso | sim | sim | sim |
| 2. Refs penduradas | não | sim | sim | sim |
| 3. EOF | silencioso | sim | sim | sim |
| 4. Seções vazias | não | sim | sim | sim |
| 5. `.gitignore` mínimo | não | não | não | não |
| 6. Wikilinks fantasma | não | sim | sim | sim |
| **7. Fluxo encerrado** | sim (destino simples) | sim | sim | sim (checklist completo) |

Notas operacionais:

- **Check 5 pulado em todos os modos do Spec Flow** — workflow não cria estrutura de projeto novo (`.gitignore` mínimo só vale para Init/Setup Project).
- **Modo Exploração silencioso** — Checks 1 e 3 rodam, mas o relatório dos checks aparece só se houver problema real. Preserva natureza do modo (captura livre, sem ritual de fechamento). **Exceção:** a confirmação do Check 7 (destino declarado) é sempre exibida (1 linha), mesmo com os demais checks silenciados.
- **Checks "não"** — não executam no modo (não consomem ciclo). Mantém Exploração leve, sem fricção desnecessária.
- **Hard-block sobre execução da skill (Decisão 05) continua valendo** — adopter pode recusar conteúdo de check específico, mas não pode pular a skill inteira. Em modo Exploração, a skill ainda roda — apenas os checks aplicáveis são silenciados.

Para os outros 7 workflows (Init Project, Setup Project, Create Feature, Refactor, Review, Handoff, Learn Simple), a matriz original continua valendo (Init/Setup rodam Checks 1-6; outros rodam Checks 1, 2, 3, 4, 6).

---

## Passos

### 1. Receber a lista de arquivos produzidos
O workflow passa para a skill a lista dos arquivos criados ou editados durante a execução. A skill opera sobre essa lista, não sobre o projeto inteiro.

### 2. Aplicar os checks aplicáveis
- Workflows Init Project e Setup Project: rodar os 6 checks (1, 2, 3, 4, 5, 6).
- Workflows Create Feature, Refactor, Review, Handoff, Learn Simple: rodar 5 checks (1, 2, 3, 4, 6).
- Workflow Cortex Spec Flow: aplicar a matriz de checks por modo (ver seção "Matriz de checks por modo do Cortex Spec Flow" acima). Check 7 sempre roda; Checks 1-6 conforme matriz.

### 3. Consolidar achados
Agrupar achados por tipo. Exemplo:

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

.gitignore mínimo — faltantes:
  Fora do Git por default:
    - .claude/
    - AGENTS.md
  Fora do Git por default, com permissão de versionar arquivo específico via regra negativa:
    - docs/
    - memory/

Wikilinks fantasma:
  - .cortex/10-padroes-tecnicos/stacks/python.md:12 → memoria-operacional (registro interno, não publicado no OSS) (sugerido: memory/project-context.md)
```

### 4. Exibir output mínimo obrigatório (sempre visível, mesmo se zero problemas)

A skill SEMPRE produz output visual ao final, independente de ter achados. Isso resolve a percepção "parece que não rodou" e dá feedback explícito ao adopter de que a etapa de fechamento foi cumprida.

**Formato com zero problemas (workflow não-Init/Setup):**

```text
🔍 Validação de encerramento — <nome do workflow>

✅ Placeholders: 0 pendentes
✅ Referências: N verificadas, 0 penduradas
✅ EOF/newline: ok
✅ Seções vazias: 0
✅ Wikilinks fantasma: 0

Workflow <nome> pode ser concluído.
```

**Formato com zero problemas (Init Project ou Setup Project):**

```text
🔍 Validação de encerramento — <nome do workflow>

✅ Placeholders: 0 pendentes
✅ Referências: N verificadas, 0 penduradas
✅ EOF/newline: ok
✅ Seções vazias: 0
✅ .gitignore mínimo: 10/10 entradas presentes
✅ Wikilinks fantasma: 0

Workflow <nome> pode ser concluído.
```

**Formato com achados:**

```text
🔍 Validação de encerramento — <nome do workflow>

❌ Placeholders: 3 pendentes (ver detalhes abaixo)
✅ Referências: 12 verificadas, 0 penduradas
✅ EOF/newline: ok
⚠ Seções vazias: 1 (ver detalhes)
❌ .gitignore mínimo: 4 entradas faltantes (ver detalhes)
✅ Wikilinks fantasma: 0

<bloco de detalhes>

Workflow <nome> NÃO pode ser declarado concluído até resolução das pendências marcadas com ❌, ou aceitação explícita registrada em docs/context/current-status.md.
```

**Formato modo Exploração do Cortex Spec Flow (checks aplicáveis limpos):**

```text
🔍 Encerramento (Exploração) — Cortex Spec Flow

✅ Fluxo encerrado: destino declarado em assinatura.md

Demais checks silenciados (modo Exploração). Workflow pode ser concluído.
```

### 5. Resolver achados ou registrar pendências
Para cada categoria com achados, perguntar:

- **Placeholders:** "Substituir agora ou seguir com pendências registradas?"
- **Referências penduradas:** para cada uma, oferecer 3 opções:
  - (a) criar o arquivo referenciado agora;
  - (b) remover a referência;
  - (c) substituir por outra fonte que exista.
- **EOF:** corrigir automaticamente é razoável (decisão do agente que invocou).
- **Seções vazias:** "Preencher agora, remover seção, ou seguir como pendência?"
- **`.gitignore` mínimo:** "Adicionar as entradas faltantes? [s/N]" (apenas Init/Setup).
- **Wikilinks fantasma:** "Substituir automaticamente conforme sugestões? [s/N]"

Pendências aceitas são registradas em `docs/context/current-status.md` do projeto do adopter, com data e descrição.

### 6. Declarar conclusão
Após o relatório ser mostrado e os achados terem sido tratados (resolvidos ou registrados como pendência aceita), o agente pode declarar o workflow concluído. Antes disso, **não**.

---

## Saída esperada

Ao final da skill deve existir:

- relatório consolidado visível ao adopter, com seção de cada um dos 6 (ou 5) checks;
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
- `.gitignore` cobre o mínimo? (Init/Setup)
- wikilink fantasma? (conforme matriz por modo no Spec Flow; sempre nos outros 7 workflows)
- fluxo encerrado com destino declarado? (somente Cortex Spec Flow)

Esta skill **não opera sobre o projeto inteiro**, apenas sobre os arquivos produzidos ou editados pelo workflow que a invocou. Operação ampla é fora do escopo.

Esta skill **não valida URLs HTTP**, apenas referências internas a arquivos locais.

Esta skill **não varre arquivos da governança** (`.cortex/`) para wikilinks fantasma — esses arquivos podem conter refs que resolvem no vault do autor e viram fantasma só no OSS público; responsabilidade do script de build, não desta skill.

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
- adopter não descobre fricção tipo "esqueci de substituir {{...}}" depois;
- `.gitignore` mínimo é aplicado em todo projeto inicializado;
- wikilinks fantasma não vazam para arquivos customizados pelo adopter;
- relatório visual sempre aparece, mesmo com zero problemas — adopter vê que a etapa rodou.

---

## Sinais de uso ruim
- skill rodada sem o usuário ver o relatório;
- workflow declarado concluído sem o relatório da skill;
- correções aplicadas automaticamente sem confirmação;
- skill expande para revisar qualidade de conteúdo (fora do escopo);
- skill opera no projeto inteiro em vez dos arquivos do workflow;
- skill varre arquivos da governança (`.cortex/`) procurando wikilinks fantasma;
- pendências registradas em arquivos paralelos em vez de `current-status.md`;
- relatório suprimido quando zero problemas — adopter precisa ver que rodou.

---

## Origem
Esta skill foi criada em 2026-05-12 para implementar as Decisões 03 (placeholders visíveis em templates) e 04 (workflows sem referências penduradas), registradas em `11-evolucao-do-sistema/02-decisoes/` a partir de fricções identificadas no teste-adopter-1.

Em 2026-05-18, recebeu três expansões coordenadas a partir do teste-adopter-2:

- **Decisão 05 (frente 9)** — torna a skill obrigatória de fato + adiciona relatório visual mínimo sempre visível.
- **Decisão 08 (frente 12)** — adiciona Check 5 (`.gitignore` mínimo) em Init/Setup Project.
- **Decisão 09 (frente 13)** — adiciona Check 6 (wikilinks fantasma) carregando lista negra de `03-skills/wikilinks-fantasma.txt`.

Em 2026-05-20, recebeu uma quarta expansão a partir da frente 11 (definição conceitual de Cortex Spec Flow, encerrada em 2026-05-19):

- **Decisão 10 (frente 14)** — adiciona Check 7 (fluxo encerrado com destino declarado) somente no [cortex-spec-flow](../08-orquestracao/tipos-de-trabalho/cortex-spec-flow.md) + matriz de checks por modo do Spec Flow (Exploração / Guiado / Conduzido / Governado). Hard-block sobre execução da skill (D05) continua valendo em todos os modos, com Exploração silenciado quando checks aplicáveis estão limpos.

---

## Regra final
O papel desta skill é simples:
**garantir que workflows que produzem documentação não fechem com pendências silenciosas — placeholders, referências penduradas, fechamentos parciais, `.gitignore` ausente, wikilinks fantasma.** Sempre obrigatória, sempre com relatório visível.

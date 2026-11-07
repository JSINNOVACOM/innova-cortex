# Innova Cortex

> AI Engineering Harness — governança, memória e fluxo.

Innova Cortex é um harness leve que transforma o Claude (e assistentes de código similares) em um colaborador disciplinado: adiciona **governança** (regras, limites, quality gates), **memória** (contexto de projeto persistente entre sessões) e **fluxo** (workflows definidos para novos projetos, features, refatorações, revisões e handoffs).

**Não é** uma biblioteca de prompts, nem um gerador de scaffold, nem um framework de agentes. É a camada de disciplina operacional que fica entre você e o assistente.

> **Status:** v0.4.0. O harness é utilizável hoje; alguns workflows são validados em pilotos, outros são experimentais. Veja [Workflows](#workflows) abaixo para o status de cada um.

---

## Por que existe

A maioria das sessões de IA com código deriva. O assistente recomeça do zero, esquece decisões anteriores, pula para implementação e produz código que não cabe no projeto. O Innova Cortex dá ao assistente um **ambiente operacional persistente**: um conjunto fixo de regras, uma camada de memória e um pequeno conjunto de workflows nomeados que ele deve seguir.

Resultado: menos deriva, menos re-explicação, mais continuidade entre sessões e entre colaboradores.

---

## Quickstart

O Innova Cortex é instalado **por projeto** usando o **modelo `.cortex/`**: a governança é copiada para uma pasta dentro do seu projeto e referenciada via path relativo. Isso mantém o projeto portável, versionável e compatível com CI/CD.

### Via CLI (recomendado)

Dentro do seu projeto, rode:

```bash
cd ~/projetos/meu-projeto
npx innova-cortex init --name "meu-projeto" --objective "objetivo em uma linha"
```

O [CLI `innova-cortex`](https://github.com/JSINNOVACOM/innova-cortex-cli) ([npm](https://www.npmjs.com/package/innova-cortex)) automatiza os passos 1-5 da instalação manual: copia só a governança para `.cortex/`, cria o `CLAUDE.md` local já preenchido, monta `docs/` e `memory/` e grava `.cortex/VERSION` com a versão instalada.

- Use `--ref <tag>` para fixar uma release (ex.: `--ref v0.4.0`).
- Sem `--name`/`--objective`, o `CLAUDE.md` fica com placeholders marcados com `<!-- TODO -->`.
- Requer Node `>= 20` e `git` no `PATH`.
- Não rode `npm install innova-cortex`: o pacote é uma CLI de instalação, não uma dependência do projeto.

Depois, siga para [Inicie o Claude](#inicie-o-claude).

### Instalação manual

Se preferir não usar o CLI, siga os passos abaixo.

#### 1. Clone o Innova Cortex em um local temporário

```bash
git clone https://github.com/<seu-usuario>/innova-cortex /tmp/cortex
```

#### 2. Vá até o seu projeto

```bash
cd ~/projetos/meu-projeto
```

#### 3. Copie a governança para `.cortex/`

```bash
mkdir -p .cortex
cp -r /tmp/cortex/cortex/* .cortex/
```

O repositório traz a governança em `cortex/` e os materiais auxiliares (`examples/`, `README.md`, `LICENSE`) na raiz. O comando acima copia **só** a governança para o `.cortex/` do seu projeto — `examples/` fica de fora.

#### 4. Crie o `CLAUDE.md` local a partir do template

```bash
cp .cortex/04-templates/template-claude-md-local.md ./CLAUDE.md
```

Abra o `CLAUDE.md` e preencha os placeholders:

- `{{PROJECT_NAME}}` → nome do projeto
- `{{PROJECT_OBJECTIVE}}` → objetivo em uma linha
- `{{CENTRAL_CLAUDE_MD_PATH}}` → `./.cortex/Claude.md`

#### 5. Crie a estrutura mínima do projeto

```bash
mkdir -p docs/context memory
```

Você também pode criar os documentos iniciais recomendados a partir dos templates em `.cortex/04-templates/`:

- `docs/as-is/system-overview.md`
- `docs/business/domain-overview.md`
- `docs/analysis/current-state-assessment.md`
- `docs/analysis/gaps-and-unknowns.md`
- `docs/context/current-status.md`
- `docs/context/handoff.md`

### Inicie o Claude

```bash
claude
```

Envie uma primeira mensagem como:

```text
Leia o CLAUDE.md local e use o fluxo Init Project para iniciar este projeto.
```

Pronto. O assistente vai ler o `CLAUDE.md`, seguir até o `.cortex/Claude.md` e operar dentro do harness.

---

## Estrutura do projeto após instalação

```text
meu-projeto/
├── .cortex/                  ← cópia da governança (regras, agentes, workflows, templates)
│   ├── 01-regras/
│   ├── 02-agentes/
│   ├── 03-skills/
│   ├── 04-templates/
│   ├── 08-orquestracao/
│   ├── 10-padroes-tecnicos/
│   └── Claude.md             ← arquivo central de orientação
├── CLAUDE.md                 ← ponte do projeto para `./.cortex/Claude.md`
├── docs/                     ← documentação viva do projeto
│   ├── as-is/
│   ├── business/
│   ├── analysis/
│   ├── context/
│   │   ├── current-status.md
│   │   └── handoff.md
│   └── design/               ← só em projeto com interface (fundação visual)
└── memory/
    └── project-context.md
```

Por que path relativo? Porque o projeto viaja junto com a governança. Quem clona o repo já tem o Innova Cortex embutido. CI/CD funciona sem setup. Sem paths específicos de máquina.

---

## Workflows

O Innova Cortex define workflows nomeados. O assistente consulta o documento correspondente em `08-orquestracao/tipos-de-trabalho/` antes de agir.

| Workflow | Frase-gatilho | Status |
|---|---|---|
| Init Project | "Use o fluxo Init Project" | Validado (4 pilotos) |
| Setup Project | "Use o fluxo Setup Project" | Em validação |
| Create Feature | "Use o fluxo Create Feature" | Em validação |
| Refactor Project | "Use o fluxo Refactor" | Sem piloto ainda |
| Review | "Use o fluxo Review" | Sem piloto ainda |
| Handoff | "Use o fluxo Handoff" | Sem piloto ainda |
| Learn Simple | "Use o fluxo Learn Simple" | Sem piloto ainda |
| Cortex Spec Flow | "Use o fluxo Cortex Spec Flow" | Auto-testado (4 modos) |

Significado dos status:

- **Validado** — usado em pilotos reais, com resultados documentados.
- **Em validação** — em andamento; existem sinais, mas conclusões ainda em aberto.
- **Auto-testado** — exercitado em auto-teste interno (todos os modos/caminhos), ainda sem adopter externo.
- **Sem piloto ainda** — definido, mas ainda não exercitado em projeto real.

Os workflows sem piloto entram porque a disciplina que eles codificam ainda é útil. Trate-os como guia, não como lei.

**Cortex Spec Flow** é opcional e diferente dos demais: é dedicado ao trabalho de spec/produto/análise (brainstorm, jornadas do usuário, regras de negócio, spec consumível por Create Feature). Tem 4 modos declarados no acionamento — **Exploração**, **Guiado** (default), **Conduzido**, **Governado** — do mais livre ao mais formal. Sem acionamento, o Cortex se comporta exatamente como antes.

### Trilho de interface

Projetos com interface seguem um trilho transversal para que **toda tela nasça coerente** com a direção visual aprovada pelo dono, com layout que faça sentido dentro do sistema do projeto. Não é um workflow novo: é a skill `03-skills/skill-trilho-de-interface.md`, acionada por Setup Project, Create Feature, Cortex Spec Flow, Review e Refactor quando a fatia cria ou muda uma tela.

- **Fundação visual** (uma vez, antes da primeira tela): brief do produto, direção aprovada em maquete, tokens, componentes e padrões de layout, registrados em `docs/design/`.
- **Nascimento de tela** (toda fatia com tela): a tela parte da fundação, tem maquete aprovada antes do código e é medida depois.
- **Modernização** (telas existentes sem fundação): plano em etapas, a partir de `04-templates/template-plano-de-interface.md`.

O trilho usa cinco ferramentas de terceiros, gratuitas, instaladas no projeto (o Cortex referencia, não empacota): Vercel `web-design-guidelines`, Taste `design-taste-frontend`, UI UX Pro Max, Impeccable e Playwright `playwright-cli`. Os critérios de medida (contraste AA, alvos de 44 px, sem rolagem lateral em 360/390/768/1440 px, claro e escuro) estão em `10-padroes-tecnicos/quality-gates/quality-gate-interface.md`.

Projetos sem interface não são afetados.

---

## O que está incluído

- `01-regras/` — regras gerais, limites de atuação, checklist de qualidade, estrutura documental do projeto
- `02-agentes/` — definições de agentes (inicialização, arquiteto, analista, planejador)
- `03-skills/` — skills focadas (iniciar projeto, mapear contexto, revisar documento, trilho de interface)
- `04-templates/` — templates mínimos para os 6 documentos iniciais + o `CLAUDE.md` local + fundação visual e plano de interface
- `08-orquestracao/` — definições de workflows, roteamento, handoffs, critérios de entrada/saída
- `10-padroes-tecnicos/` — padrões técnicos opinativos (stacks, componentes, arquitetura, quality gates)

> **Nota sobre `10-padroes-tecnicos/`:** os padrões técnicos ainda são mínimos nesta fase (Java/Spring, React/TypeScript, mais páginas genéricas de componente e arquitetura). A cobertura vai crescer em versões futuras, **dirigida por você**, não extraída automaticamente de projetos arbitrários. Se uma stack faltar, use as páginas existentes como forma e defina a sua.

---

## O que **não** é

- **Não é biblioteca de prompts.** Não há templates de prompt aqui. O harness molda como o assistente *opera*, não o que digitar para ele.
- **Não é gerador de scaffold.** Não cria código de aplicação. O CLI `innova-cortex` apenas instala o `.cortex/` no seu projeto.
- **Não é framework de agentes.** Os agentes em `02-agentes/` são definições de papel para o Claude assumir, não runners autônomos.
- **Não é específico de framework.** O layout do projeto é opinativo; a stack tecnológica não é.

---

## Roadmap

| Versão | Foco | Status |
|---|---|---|
| v0.1 | Governança, agentes, workflows e templates publicados | Publicado |
| v0.1.1 | Skill `skill-validar-encerramento` + validação de encerramento (Decisões 03/04) | Publicado |
| v0.2.0 | Cortex Spec Flow (4 modos) + skill de encerramento obrigatória + agentes proativos | Publicado |
| v0.3.0 | Protocolo de encerramento robusto em 3 níveis (Decisão 11): Check 8 (marcadores de conflito de merge), coerência interna do handoff, sugestões de clean/re-sync da fundação | Publicado |
| v0.3.1 | Novos padrões técnicos de stack: `python` (reescrita para APIs de larga escala) e `nextjs` (App Router, TypeScript) | Publicado |
| v0.4.0 | Trilho de interface: telas nascem da fundação visual (skill, `quality-gate-interface`, templates de fundação e de plano, ganchos em 6 workflows, `docs/design/`); Check 5 aceita `docs/*` e `memory/*` | Publicado |
| Próximo exemplo | Segundo projeto-exemplo (Next.js) | Planejado |
| CLI `innova-cortex` | `npx innova-cortex init` automatiza os passos 1-5 do Quickstart ([innova-cortex-cli](https://github.com/JSINNOVACOM/innova-cortex-cli)) | Publicado (npm `0.2.1`) |
| Futuro | Mais padrões técnicos, workflows validados, padrões opinativos | A definir |

O CLI é o caminho recomendado de instalação; o fluxo manual do Quickstart continua suportado.

---

## Examples

Há um pequeno projeto de referência em `examples/projeto-fastapi-ficticio/` (FastAPI). É um **projeto fictício didático** — leia para ver como fica uma instalação completa do Innova Cortex de ponta a ponta. **Não copie como scaffold.** Rode o Quickstart no seu próprio projeto e consulte o exemplo apenas como referência de layout.

Um segundo exemplo (Next.js) está planejado.

---

## Idioma

Este README e a documentação interna estão em **português brasileiro**. O harness foi pensado primeiro para devs solo, freelancers e educadores brasileiros. Versão em inglês pode ser adicionada em release futura conforme demanda.

---

## Convenção de wikilinks

Os arquivos do `.cortex/` usam wikilinks (`[[nome-do-no]]`) para referência cruzada entre regras, agentes, skills e workflows. Use **apenas wikilinks que resolvem dentro do `.cortex/` instalado**.

Se você customizar arquivos em `10-padroes-tecnicos/` (criar `python.md`, `go.md` etc.) ou criar qualquer documento novo no `.cortex/`, evite os wikilinks abaixo — eles pertencem à governança privada do autor original e **não funcionam fora do vault dele**:

| Wikilink proibido | Use em vez disso |
|---|---|
| `[[memoria-operacional]]` | `memory/project-context.md` |
| `[[pendencias]]` | seção em `docs/context/current-status.md` |
| `[[hipoteses-abertas]]` | `docs/analysis/gaps-and-unknowns.md` |
| `[[fatos-confirmados]]` | `docs/analysis/current-state-assessment.md` |

A skill `skill-validar-encerramento` (Check 6) alerta automaticamente sobre essas refs em arquivos produzidos por qualquer workflow — então o caminho é "se o aviso aparecer, substituir conforme a tabela acima".

Se algum desses refs aparecer em arquivo do `.cortex/` instalado (em vez de em algo customizado por você), é resíduo de versão antiga — abra issue no repo.

---

## Contribuição

O Innova Cortex está em release pública early-stage. As contribuições mais úteis hoje são:

1. **Rode o Quickstart** em um projeto real e abra uma issue descrevendo o que quebrou ou pareceu estranho.
2. **Execute um workflow** de ponta a ponta e relate se a frase-gatilho, o handoff e os critérios de saída funcionaram.
3. **Sugira um workflow faltante** — mas traga um exemplo da situação que ele resolveria, não uma ideia abstrata.

Pull requests para workflows, padrões técnicos ou regras são bem-vindos, mas serão revisados contra os princípios do projeto (ver `01-regras/`). Deriva no sentido de "mais regras" é resistida por design.

---

## Licença

MIT. Ver `LICENSE`.

Você pode fazer fork, adaptar e usar o Innova Cortex comercialmente. Atribuição é apreciada, mas não obrigatória.

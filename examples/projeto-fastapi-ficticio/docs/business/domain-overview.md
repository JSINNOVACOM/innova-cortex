# Domain Overview — livraria-comunitaria

## Metadados
- **Projeto:** livraria-comunitaria
- **Documento:** domain-overview
- **Estado:** inicial
- **Última atualização:** 2026-05-06
- **Responsável:** autor do projeto

---

## 1. Visão de domínio

Domínio: empréstimo de livros em uma biblioteca comunitária pequena, mantida voluntariamente por uma vizinha do bairro, sem fins lucrativos.

Não é uma biblioteca municipal. Não é uma livraria. É um arranjo informal de compartilhamento entre residentes de uma rua/quadra.

---

## 2. Problema que o projeto aparenta resolver

A bibliotecária hoje controla tudo em uma planilha de Excel. A planilha:
- depende de ela lembrar de atualizar
- não tem histórico confiável
- já levou a 3 livros "sumirem" (um vizinho devolveu mas ninguém marcou; o livro reapareceu na estante mas continuou listado como emprestado)
- não responde rápido à pergunta "quem está com o livro X agora?"

A API resolve esse subconjunto: registro confiável, consulta rápida.

---

## 3. Objetivos de negócio aparentes

- Reduzir esforço diário da bibliotecária para registrar empréstimos
- Eliminar perda de livros por falha de registro
- Permitir consulta rápida sobre status de cada livro
- Manter histórico de quem emprestou o quê (memória do acervo)

---

## 4. Atores envolvidos

- **Bibliotecária voluntária** — única operadora; cadastra, registra, consulta
- **Vizinhos cadastrados** — não acessam o sistema; são consultados por meio da bibliotecária
- **Autor do projeto** — desenvolve e mantém; também é vizinho

---

## 5. Conceitos centrais do domínio

- **Livro** — item físico no acervo, com identificação única
- **Vizinho** — pessoa autorizada a pegar livros emprestados
- **Empréstimo** — vínculo entre um livro e um vizinho, com data de início, data prevista de devolução e (eventualmente) data real de devolução
- **Acervo** — conjunto de livros cadastrados
- **Empréstimo em aberto** — empréstimo cuja data real de devolução ainda não foi registrada

---

## 6. Fluxos ou operações principais percebidas

- Cadastrar novo livro no acervo
- Cadastrar novo vizinho
- Registrar empréstimo (vizinho leva livro)
- Registrar devolução (vizinho devolve livro)
- Consultar empréstimos em aberto
- Consultar histórico de um livro específico
- Consultar histórico de um vizinho específico

---

## 7. Regras de negócio percebidas

### Confirmadas
- Um livro só pode estar emprestado para um vizinho por vez
- Um vizinho pode ter múltiplos livros emprestados simultaneamente

### Inferidas
- Prazo padrão de empréstimo é 14 dias
- Vizinhos com livros muito atrasados não devem poder pegar novos empréstimos — **a confirmar com a bibliotecária**

---

## 8. Pontos ainda não compreendidos

- Como a bibliotecária quer tratar livros não devolvidos por mais de 60 dias?
- Há limite de quantos livros um vizinho pode ter ao mesmo tempo?
- Vizinhos podem se "descadastrar" / o registro deve ser preservado?

---

## 9. Confirmado

- Operação por uma pessoa só
- Sem multas ou penalidades monetárias
- Sem reservas (chega-pega-leva)

---

## 10. Hipóteses

- O cadastro de vizinhos pode crescer com o boca-a-boca; não é estável
- Bibliotecária consulta o sistema mais do que escreve nele

---

## 11. Dúvidas em aberto

- O que fazer com livros perdidos ou danificados? Excluir? Marcar?
- Há outros vizinhos interessados em ajudar a operar a biblioteca?

---

## 12. Documentos relacionados no projeto
- `docs/as-is/system-overview.md`
- `docs/analysis/current-state-assessment.md`
- `docs/analysis/gaps-and-unknowns.md`
- `docs/context/current-status.md`
- `docs/context/handoff.md`

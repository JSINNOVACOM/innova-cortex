# System Overview — livraria-comunitaria

## Metadados
- **Projeto:** livraria-comunitaria
- **Documento:** system-overview
- **Estado:** inicial
- **Última atualização:** 2026-05-06
- **Responsável:** autor do projeto

---

## 1. Resumo do projeto

API REST de apoio à biblioteca comunitária do bairro. Permite à bibliotecária voluntária registrar o acervo, controlar empréstimos entre vizinhos cadastrados e localizar livros emprestados.

Projeto pessoal. Sem prazo comercial. Sem usuários reais ainda.

---

## 2. Propósito aparente

Substituir a planilha que a bibliotecária mantém hoje (manual, frágil, sem histórico) por um registro consultável.

O foco não é "construir um sistema completo de biblioteca", e sim **resolver o problema operacional concreto da bibliotecária**: saber quem pegou qual livro, quando precisa devolver, e quem pode pegar emprestado.

---

## 3. O que o sistema aparentemente faz

- Cadastrar livro (título, autor, ano, status)
- Cadastrar vizinho (nome, contato, endereço de referência)
- Registrar empréstimo (livro + vizinho + data)
- Registrar devolução (empréstimo + data de retorno)
- Listar empréstimos em aberto (livros que ainda não voltaram)

---

## 4. Áreas principais identificadas

- **Acervo** — livros cadastrados, status atual
- **Cadastro** — vizinhos com permissão de empréstimo
- **Empréstimos** — vínculo livro-vizinho com janela temporal
- **Consulta** — listagem operacional para a bibliotecária

---

## 5. Limites percebidos

### Dentro do escopo percebido
- Cadastro de livros e vizinhos
- Registro de empréstimos e devoluções
- Listagem de empréstimos em aberto
- Operação por uma única pessoa (a bibliotecária)

### Fora do escopo percebido
- Reservas antecipadas
- Multas ou cobrança
- Sistema de busca avançada / catálogo público
- Aplicativo mobile
- Múltiplas bibliotecárias com perfis distintos
- Notificações automáticas de prazo

---

## 6. Fontes usadas para esta visão

- Conversas iniciais com a bibliotecária voluntária
- Planilha atual (Excel) usada hoje
- Lista informal de vizinhos cadastrados (papel)

---

## 7. Confirmado

- A bibliotecária é a única operadora prevista por enquanto
- Acervo atual gira em torno de 80 livros
- Não há requisito de autenticação multiusuário neste momento

---

## 8. Hipóteses

- Empréstimo padrão dura 14 dias — ainda não validado formalmente
- Lista de vizinhos cadastrados será migrada manualmente da lista em papel

---

## 9. Dúvidas em aberto

- Como tratar livros perdidos ou danificados?
- Há vizinhos suspensos por atraso recorrente? Como representar?

---

## 10. Observações relevantes

- O projeto é pequeno. Resistir à tentação de adicionar features genéricas de "sistema de biblioteca".
- O critério de sucesso é **a bibliotecária parar de usar a planilha**, não "ter um sistema completo".

---

## 11. Documentos relacionados no projeto
- `docs/business/domain-overview.md`
- `docs/analysis/current-state-assessment.md`
- `docs/analysis/gaps-and-unknowns.md`
- `docs/context/current-status.md`
- `docs/context/handoff.md`

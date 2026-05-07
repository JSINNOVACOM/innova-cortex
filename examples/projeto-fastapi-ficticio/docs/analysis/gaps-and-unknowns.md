# Gaps and Unknowns — livraria-comunitaria

## Metadados
- **Projeto:** livraria-comunitaria
- **Documento:** gaps-and-unknowns
- **Estado:** inicial
- **Última atualização:** 2026-05-06
- **Responsável:** autor do projeto

---

## 1. Resumo das lacunas

Lacunas pequenas e tratáveis. Nenhuma é bloqueante para começar a implementação do MVP. Duas merecem atenção antes de adicionar features além do MVP.

---

## 2. Lacunas prioritárias

### Lacuna 1
- **Descrição:** falta definição explícita do MVP (lista das operações mínimas para a bibliotecária trocar a planilha pelo sistema).
- **Por que isso importa:** sem essa definição, qualquer escopo cresce indefinidamente.
- **O que bloqueia:** decisão de "quando parar de adicionar features na primeira versão".
- **Como investigar:** sentar com a bibliotecária, perguntar quais 3-4 ações ela mais faz na planilha.
- **Prioridade:** alta

### Lacuna 2
- **Descrição:** falta plano de migração da planilha atual para o novo sistema.
- **Por que isso importa:** sem migração definida, ela mantém os dois em paralelo (situação pior).
- **O que bloqueia:** entrar em produção real.
- **Como investigar:** ver o formato da planilha, decidir entre import via script vs digitação manual de 80 livros.
- **Prioridade:** média (não bloqueia o MVP técnico, mas bloqueia a adoção real)

### Lacuna 3
- **Descrição:** tratamento de livros perdidos ou nunca devolvidos.
- **Por que isso importa:** acervo real tem livros que sumiram. Modelo precisa lidar com isso.
- **O que bloqueia:** schema final do `Empréstimo` (precisa de status "perdido"? ou só de uma flag no `Livro`?).
- **Como investigar:** perguntar à bibliotecária como ela trata isso hoje na planilha.
- **Prioridade:** baixa para o MVP; alta para evolução

---

## 3. Dúvidas em aberto

- Vizinhos podem ser bloqueados temporariamente? Como?
- Há livros doados que não devem ser emprestados (apenas para leitura no local)?
- O sistema precisa rodar offline em algum momento?

---

## 4. Hipóteses em validação

- A bibliotecária topa usar uma interface CLI ou Postman para registrar empréstimos no início (sem UI web)
- Manter histórico completo de empréstimos passados é desejável (não só os abertos)

---

## 5. Bloqueios atuais para avanço seguro

Nenhum bloqueio absoluto.

Avisos: avançar para implementação sem fechar a Lacuna 1 (definição do MVP) cria risco alto de scope creep.

---

## 6. Ordem sugerida de investigação

1. Fechar Lacuna 1 (definição do MVP) — conversa de 30 minutos com a bibliotecária
2. Esboçar schema das 3 entidades em uma sessão dedicada
3. Implementar MVP em 1-2 finais de semana
4. Após 2 semanas de uso real, retomar Lacuna 2 e Lacuna 3

---

## 7. Confirmado

- Não é necessário autenticação multiusuário no MVP
- Não é necessário interface web no MVP

---

## 8. Registros globais do sistema

Nenhum impacto transversal no sistema da IA identificado neste momento.

---

## 9. Documentos relacionados no projeto
- `docs/as-is/system-overview.md`
- `docs/business/domain-overview.md`
- `docs/analysis/current-state-assessment.md`
- `docs/context/current-status.md`
- `docs/context/handoff.md`

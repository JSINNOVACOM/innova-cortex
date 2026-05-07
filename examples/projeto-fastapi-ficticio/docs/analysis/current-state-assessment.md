# Current State Assessment — livraria-comunitaria

## Metadados
- **Projeto:** livraria-comunitaria
- **Documento:** current-state-assessment
- **Estado:** inicial
- **Última atualização:** 2026-05-06
- **Responsável:** autor do projeto

---

## 1. Resumo da avaliação

Projeto em estágio embrionário. Domínio claro e pequeno, com regras de negócio simples. O risco não é técnico — é de **scope creep**: tentação de virar um "sistema de biblioteca" completo em vez de resolver o problema concreto da planilha.

---

## 2. Nível atual de clareza

- **Nível percebido:** alto
- **Justificativa:** o domínio é tão pequeno que cabe em um diagrama mental simples (livro, vizinho, empréstimo). Não há ambiguidade significativa nas operações principais.

---

## 3. Qualidade da documentação existente

- **Situação percebida:** apenas os 6 docs iniciais do Innova Cortex foram preenchidos. Não há documentação técnica, código mínimo ou wireframes ainda.
- **Pontos fortes:** o problema está bem entendido; a bibliotecária está disponível para validar dúvidas.
- **Pontos fracos:** nenhum desenho de schema feito; nenhuma decisão registrada sobre stack até agora (decisão informal: FastAPI + SQLAlchemy + SQLite).

---

## 4. Dependência de conhecimento informal

Alta — quase todo o conhecimento operacional está na cabeça da bibliotecária. Se ela parar de operar, o sistema perde o contexto vivo. Mitigação: o cadastro de vizinhos e o histórico de empréstimos passam a ser registros persistidos.

---

## 5. Fragilidades percebidas

- Risco de scope creep (descrito no item 1)
- Stack inicial pode ser excessiva para 80 livros e 1 operadora — mas é a stack que o autor conhece
- Não há plano de backup do banco SQLite ainda
- Sem testes automatizados; sem ambiente de staging

---

## 6. Áreas mais críticas

- Definir o **mínimo viável** que faz a bibliotecária parar de usar a planilha
- Definir como migrar a planilha atual para o novo sistema
- Decidir o tratamento de livros perdidos ou nunca devolvidos

---

## 7. Impactos percebidos das fragilidades

- Sem MVP claro → projeto vira interminável
- Sem plano de migração → bibliotecária mantém duas fontes em paralelo (a planilha e o novo sistema), o que é pior que só ter a planilha
- Sem backup → uma falha de disco apaga 80 livros e 1 ano de histórico

---

## 8. Confirmado

- A bibliotecária topa migrar quando o sistema estiver "consultável e registrável"
- O projeto não tem prazo

---

## 9. Hipóteses

- 1-2 finais de semana de codificação focada chegam ao MVP
- Backup pode ser resolvido com cron simples copiando o `.sqlite` para outra pasta

---

## 10. Dúvidas em aberto

- Qual o critério mínimo para considerar o sistema "pronto para começar a substituir a planilha"?
- Vale fazer interface web ou só endpoints REST + curl/Postman para a bibliotecária?

---

## 11. Recomendação inicial

1. Definir explicitamente o MVP (listar as 3-4 operações que a bibliotecária precisa para parar de usar a planilha).
2. Modelar `Livro`, `Vizinho` e `Empréstimo` em uma única sessão.
3. Implementar as operações do MVP.
4. **Antes de adicionar qualquer feature além do MVP**, fazer a bibliotecária usar por 2 semanas e coletar fricções reais.

---

## 12. Documentos relacionados no projeto
- `docs/as-is/system-overview.md`
- `docs/business/domain-overview.md`
- `docs/analysis/gaps-and-unknowns.md`
- `docs/context/current-status.md`
- `docs/context/handoff.md`

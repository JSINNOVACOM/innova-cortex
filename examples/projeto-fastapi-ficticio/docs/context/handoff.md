# Handoff — livraria-comunitaria

## Metadados
- **Projeto:** livraria-comunitaria
- **Documento:** handoff
- **Estado:** inicial
- **Última atualização:** 2026-05-06
- **Responsável:** autor do projeto

---

## 1. Onde o trabalho parou

Os 6 documentos iniciais (system-overview, domain-overview, current-state-assessment, gaps-and-unknowns, current-status, handoff) foram criados via fluxo Init Project. Nenhum código ainda. Aguardando conversa com a bibliotecária para fechar a Lacuna 1 (definição do MVP).

---

## 2. Última frente analisada

Mapeamento das lacunas em `docs/analysis/gaps-and-unknowns.md`. Foram identificadas 3 lacunas prioritárias; nenhuma é bloqueante absoluta, mas a Lacuna 1 (MVP) é pré-requisito para iniciar implementação com escopo controlado.

---

## 3. O que já está pronto

- Os 6 docs iniciais preenchidos e consistentes entre si
- Domínio compreendido em alto nível
- Stack escolhida (FastAPI + SQLAlchemy + SQLite)
- Posicionamento do projeto: pequeno, focado, sem scope creep

---

## 4. O que ainda precisa acontecer

- Reunião com a bibliotecária para definir MVP
- Schema das 3 entidades
- Decisão sobre interface de uso da bibliotecária no início (CLI? Postman?)
- Decisão sobre como migrar a planilha atual
- Implementação do MVP
- Ciclo de uso real de 2 semanas antes de qualquer feature além do MVP

---

## 5. Próximos passos imediatos

1. Marcar 30 minutos com a bibliotecária — perguntar quais 3-4 ações ela mais faz na planilha
2. Atualizar `current-state-assessment.md` e `current-status.md` com a resposta crua
3. Em sessão separada, modelar as 3 entidades e registrar decisão na pasta `docs/decisions/` (a ser criada se necessário)

---

## 6. Alertas importantes

- **Não codar sem fechar a Lacuna 1.** Risco de scope creep está explicitamente registrado no `current-state-assessment.md`.
- A stack escolhida (FastAPI + SQLAlchemy + SQLite) **é decisão do autor**. Não foi prescrita pelo Innova Cortex e pode ser revista — o domínio comportaria opções mais leves (ex.: arquivo `.json` no início), mas o autor optou pela stack que conhece.
- A bibliotecária é a única fonte sobre o domínio real — sem ela validando, qualquer hipótese sobre regras de negócio fica em aberto.

---

## 7. Pontos que ainda exigem validação

- MVP definido (Lacuna 1)
- Tratamento de livros perdidos (Lacuna 3)
- Limite de livros simultâneos por vizinho (Hipótese)
- Prazo padrão de 14 dias (Hipótese)

---

## 8. Materiais que devem ser consultados primeiro

- `docs/as-is/system-overview.md`
- `docs/business/domain-overview.md`
- `docs/analysis/current-state-assessment.md`
- `docs/analysis/gaps-and-unknowns.md`
- `docs/context/current-status.md`

---

## 9. Observações finais para continuidade

Quando retomar:
1. Ler `current-status.md` primeiro (estado vivo)
2. Confirmar se a reunião com a bibliotecária aconteceu
3. Se sim: atualizar Lacuna 1 e seguir para schema
4. Se não: a próxima ação continua sendo marcar essa reunião — nada mais

---

## 10. Registros auxiliares no sistema

Nenhum impacto global no sistema da IA neste momento.

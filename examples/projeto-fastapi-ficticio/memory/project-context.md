# Memória do projeto — livraria-comunitaria

## Propósito deste arquivo

Este arquivo é a **memória persistente** deste projeto. Diferente dos `docs/`, que são documentação estruturada para humanos, esta memória existe para que o agente (Claude) carregue contexto operacional rapidamente em sessões futuras.

Não duplicar aqui o que já está nos `docs/`. Registrar apenas o que ajuda a próxima sessão a começar bem.

---

## Identidade do projeto

- **Nome:** livraria-comunitaria
- **Tipo:** API REST de uso pessoal/comunitário
- **Stack:** FastAPI + SQLAlchemy + SQLite (escolha do autor)
- **Estado:** iniciação concluída; aguardando definição do MVP

---

## Decisões já tomadas

- **Modelo de instalação:** Modelo B (`.cortex/` dentro do projeto, path relativo)
- **Idioma da documentação:** PT-BR
- **Linguagem da API:** Python
- **Banco:** SQLite no início; revisitar se cresce
- **Não fazer no MVP:** autenticação, UI web, reservas, multas, notificações

---

## Restrições conhecidas

- Operadora única (a bibliotecária)
- Acervo pequeno (~80 livros)
- Sem prazo comercial
- Sem orçamento; tempo do autor é o único recurso

---

## Pessoas-chave

- **A bibliotecária voluntária** — fonte única do domínio real. Disponível para validar dúvidas mediante aviso prévio.
- **O autor** — desenvolve, mantém, e também é vizinho cadastrado da biblioteca.

---

## O que evitar nas próximas sessões

- Sugerir features genéricas de "sistema de biblioteca" (multas, reservas, perfis, notificações) — explicitamente fora do MVP
- Trocar a stack sem motivo concreto
- Codar antes de fechar a Lacuna 1 (MVP)
- Tratar hipóteses como confirmadas (ex.: prazo de 14 dias é hipótese, não regra)

---

## O que privilegiar

- Manter o projeto **pequeno**
- Validar com a bibliotecária antes de assumir regras de negócio
- Registrar mudanças em `docs/context/current-status.md` ao fim de cada sessão
- Atualizar este arquivo apenas se uma decisão duradoura for tomada

---

## Última sessão

- **Data:** 2026-05-06
- **O que aconteceu:** fluxo Init Project executado; 6 docs iniciais preenchidos
- **Próxima ação registrada:** marcar 30 min com a bibliotecária para definir MVP

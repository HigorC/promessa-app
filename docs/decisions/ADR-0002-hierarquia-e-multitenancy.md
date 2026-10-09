# ADR-0002 — Hierarquia institucional e multi-tenancy

- Estado: aceita para planejamento
- Data: 2026-10-05

## Contexto

A primeira implantação atenderá uma convenção da Igreja Adventista da Promessa no Brasil. O produto deve poder crescer para outras convenções, países e organizações religiosas sem uma reescrita integral.

## Decisão

Modelar separadamente:

- país;
- organização religiosa/denominação;
- unidade administrativa, inicialmente do tipo convenção;
- igreja local.

A organização é o limite principal de tenant. A igreja é o limite operacional mais frequente. Pessoas podem possuir vínculos e papéis diferentes por igreja ou unidade.

Não codificar “Promessa”, “Brasil” ou “Convenção Sul-Mato-Grossense” como constantes que definem autorização ou filtragem.

## Justificativa

- Evita misturar marca/denominação com igreja física.
- Permite adicionar convenções da mesma organização.
- Permite organizações com nomes diferentes para suas divisões regionais.
- Torna RLS e delegação administrativa explícitas.
- Evita o antipadrão de um único `role` global no usuário.

## Consequências

- O esquema inicial terá mais entidades do que uma solução exclusiva para uma igreja.
- Toda consulta operacional precisa carregar ou derivar contexto de organização/igreja.
- Seeds e testes devem incluir pelo menos duas organizações e igrejas distintas para provar isolamento.
- Configuração de marca e vocabulário pertence à organização.

## Limite contra abstração prematura

O modelo suporta outros nomes de unidade, mas não tentará representar toda estrutura eclesiástica possível no primeiro MVP. Novos níveis só serão introduzidos com caso real.


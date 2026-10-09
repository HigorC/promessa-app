# Índice da documentação

## Como usar este diretório

Os documentos numerados devem ser lidos na ordem quando um agente ainda não conhece o projeto. Para tarefas específicas, use a tabela abaixo.

| Documento | Assunto | Estado |
|---|---|---|
| [01-contexto-e-visao.md](01-contexto-e-visao.md) | Problema, público, escopo e princípios | Contexto confirmado + hipóteses marcadas |
| [02-dominio-e-dados.md](02-dominio-e-dados.md) | Hierarquia institucional e entidades | Modelo conceitual proposto |
| [03-arquitetura.md](03-arquitetura.md) | Stack, fronteiras e estrutura futura | Direção arquitetural escolhida para planejamento |
| [04-identidade-acesso-e-privacidade.md](04-identidade-acesso-e-privacidade.md) | CPF, Google/Apple, papéis, RLS e LGPD | Requisitos confirmados + fluxo proposto |
| [05-offline-e-notificacoes.md](05-offline-e-notificacoes.md) | Cache, sincronização e push | Estratégia inicial proposta |
| [06-aproveitamento-do-legado.md](06-aproveitamento-do-legado.md) | Inventário e regras para migração seletiva | Levantamento inicial |
| [07-roadmap-e-questoes-abertas.md](07-roadmap-e-questoes-abertas.md) | Fases, MVP e decisões pendentes | Vivo |
| [glossario.md](glossario.md) | Vocabulário comum | Vivo |

## Decisões arquiteturais

| ADR | Decisão | Estado |
|---|---|---|
| [ADR-0001](decisions/ADR-0001-stack-expo-supabase.md) | Expo/React Native + Supabase | Aceita para planejamento |
| [ADR-0002](decisions/ADR-0002-hierarquia-e-multitenancy.md) | Organização como limite de tenant e igreja como limite operacional | Aceita para planejamento |
| [ADR-0003](decisions/ADR-0003-cpf-e-vinculo-de-identidades.md) | CPF como identificador, nunca como autenticador isolado | Aceita para planejamento |

## Rotas rápidas para agentes

- **Nova tela ou fluxo de usuário:** contexto, domínio e documento específico da funcionalidade.
- **Banco ou API:** domínio, arquitetura, autenticação e ADR-0002.
- **Login ou permissões:** documento 04 e ADR-0003.
- **Push ou funcionamento sem internet:** documento 05.
- **Reaproveitar algo do projeto anterior:** documento 06 antes de ler/copiar código.
- **Mudar uma tecnologia central:** documento 03 e novo ADR.

## Regra de autoridade

Em caso de conflito:

1. solicitação mais recente e explícita do responsável do produto;
2. ADR aceito mais recente;
3. documentos numerados;
4. comentários e código existente.

Se o conflito não puder ser resolvido com essas fontes, registre a dúvida e peça decisão.


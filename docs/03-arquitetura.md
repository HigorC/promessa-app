# Arquitetura proposta

## Objetivos arquiteturais

- Uma base de produto para Android, iOS e web.
- Push remoto como capacidade de primeira classe.
- Leitura offline de conteúdo já sincronizado.
- Backend gerenciado, sem operação de servidores pela equipe.
- Isolamento seguro entre organizações e igrejas.
- Evolução rápida de esquema com histórico e compatibilidade.
- Estrutura simples o suficiente para uma equipe pequena e para agentes de IA.

## Stack escolhida para planejamento

### Cliente

- Expo e React Native.
- TypeScript estrito.
- Expo Router.
- Development builds desde o início; Expo Go não será o ambiente principal.
- TanStack Query para estado remoto.
- Expo SQLite para persistência local estruturada.
- Expo Notifications para registro e recebimento de push.
- Secure Store para material sensível apropriado ao dispositivo.

### Backend gerenciado

- Supabase Auth.
- PostgreSQL do Supabase.
- Row Level Security.
- Supabase Storage.
- Edge Functions para operações privilegiadas e integrações.
- Webhooks/Cron conforme os casos de notificação e lembrete.

### Operação futura

- EAS Build/Submit como conveniência inicial, sem tornar regras de negócio dependentes de EAS.
- Provedor de hospedagem web a decidir.
- Sentry ou equivalente para erros, desempenho e releases.
- CI com lint, TypeScript, testes, validação de migrations e políticas de autorização.

Detalhes e justificativa estão no [ADR-0001](decisions/ADR-0001-stack-expo-supabase.md).

## Fronteiras lógicas

```text
Interface (mobile/web)
        │
Casos de uso / serviços de domínio
        │
Repositórios e adaptadores
   ┌────┴─────────┐
Cache/SQLite   Supabase API/RPC
                     │
        PostgreSQL + RLS + Edge Functions
                     │
          Outbox → serviço de push
```

Regras importantes:

- componentes visuais não devem conter regras de autorização;
- telas não devem consultar tabelas de forma desorganizada;
- serviços de domínio expõem ações como `publishEvent`, `assignMember` e `changeMemberRole`;
- RLS continua sendo a última barreira, independentemente das validações do cliente;
- operações administrativas privilegiadas vivem em função segura ou RPC cuidadosamente autorizada;
- o cliente nunca recebe chave privilegiada.

## Estrutura futura sugerida

Evitar monorepo e múltiplos aplicativos até existir necessidade real. Uma estrutura inicial possível:

```text
app/                  rotas Expo Router
src/
  features/           módulos por capacidade de negócio
  domain/             tipos e regras sem dependência visual
  data/               repositórios, Supabase e cache
  components/         componentes compartilhados
  design-system/      tokens e componentes básicos
  lib/                infraestrutura pequena e transversal
supabase/
  migrations/
  functions/
  seed.sql
tests/
docs/
```

Cada feature deve possuir uma superfície clara e, quando crescer, seus próprios componentes, hooks, schemas e testes. Não criar pastas genéricas gigantes como `utils` ou `services` sem domínio.

## Web

- A experiência pública precisa usar URLs compartilháveis.
- Páginas de igreja e eventos publicados devem renderizar conteúdo acessível e navegável.
- O painel administrativo pode ter componentes específicos para web quando tabelas, teclado e telas grandes justificarem.
- Não perseguir compartilhamento de 100% do código visual; compartilhar domínio, validação e acesso a dados é mais importante.

## Banco e contratos

- Toda alteração por migration versionada no repositório.
- Ambientes local, homologação e produção separados.
- Tipos TypeScript gerados a partir do esquema.
- Seed sem dados pessoais reais.
- Views/RPCs podem estabilizar contratos de leitura complexos.
- Mudanças destrutivas exigem ciclo de depreciação.
- RLS deve ser testada com usuários de organizações e igrejas diferentes.

### Padrão de alteração compatível

1. Adicionar coluna/tabela/contrato novo de forma opcional.
2. Publicar backend capaz de atender clientes antigos e novos.
3. Publicar o cliente que usa o novo contrato.
4. Preencher/migrar dados antigos.
5. Observar versões ainda ativas.
6. Endurecer constraint ou remover contrato antigo em entrega posterior.

## Serverless: significado adotado

O produto utilizará serviços gerenciados e funções sob demanda, sem que a equipe mantenha VMs ou servidores de aplicação. PostgreSQL continua sendo uma instância de banco gerenciada; “serverless” descreve o modelo operacional, não a inexistência de servidores.

## Escala

O volume inicial de cerca de 350 membros é pequeno para qualquer stack proposta. As prioridades arquiteturais são clareza, segurança, custo previsível e capacidade de evolução. Otimizações de grande escala só devem ser introduzidas com métricas.

## Referências técnicas

- [React Native recomenda um framework como Expo](https://reactnative.dev/docs/next/environment-setup)
- [Expo development builds](https://docs.expo.dev/develop/development-builds/introduction/)
- [Expo push notifications](https://docs.expo.dev/push-notifications/push-notifications-setup/)
- [Supabase para Expo/React Native](https://supabase.com/docs/guides/getting-started/quickstarts/expo-react-native)
- [Supabase migrations](https://supabase.com/docs/guides/local-development/database-migrations)
- [Supabase RLS](https://supabase.com/docs/guides/database/postgres/row-level-security)


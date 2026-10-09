# ADR-0001 — Expo/React Native e Supabase

- Estado: aceita para planejamento
- Data: 2026-10-05

## Contexto

O produto precisa atender Android, iOS e web, receber push remoto, oferecer leitura offline, suportar contas e permissões multi-igreja e ser mantido por uma equipe pequena. A migração a partir do aplicativo atual não é restrição para a escolha.

## Decisão

Adotar como direção:

- Expo/React Native e TypeScript no cliente;
- Expo Router para navegação universal;
- Expo Notifications para integração de push;
- SQLite + TanStack Query para cache local;
- Supabase Auth, PostgreSQL, RLS, Storage e Edge Functions no backend.

Usar development builds desde o início. EAS é conveniência operacional, não dependência das regras do domínio.

## Justificativa

- Expo é framework de produção recomendado no ecossistema React Native.
- Possui caminho direto para Android, iOS, web e notificações.
- Permite código nativo quando necessário.
- PostgreSQL representa naturalmente as relações entre organização, convenção, igreja, pessoa, evento e escala.
- RLS oferece uma barreira adequada para dados multi-tenant.
- Migrations e tipos gerados tornam a evolução de contratos explícita.

## Alternativas consideradas

### Flutter

Excelente para mobile, mas a experiência web baseada em conteúdo e DOM é menos natural. Poderia exigir um segundo frontend web.

### Capacitor

Boa opção web-first, com acesso nativo, mas menos alinhada à prioridade de experiência mobile e ao fluxo desejado de Expo/React Native.

### Tauri 2

Boa opção para desktop/webview. O ecossistema mobile e o caminho de push remoto exigiriam mais integração nativa para uma capacidade central do produto.

### Aplicativos nativos separados

Oferecem máximo controle, mas duplicam esforço sem necessidade demonstrada para a escala e complexidade atuais.

### Firebase

Possui autenticação, push e offline fortes, mas o domínio relacional, relatórios e autorização por múltiplos escopos favorecem PostgreSQL/Supabase.

## Consequências

- Parte da UI web poderá precisar de arquivos/componentes específicos.
- Alterações nativas exigirão novos builds.
- A equipe precisa dominar RLS e testá-la como parte do produto.
- Offline será implementado deliberadamente, não obtido automaticamente para todo o domínio.
- Push continuará sujeito às limitações de entrega dos sistemas operacionais.

## Critérios para revisitar

- Necessidade comprovada de dois frontends independentes.
- Requisitos nativos que Expo/React Native não consiga atender razoavelmente.
- Edição offline colaborativa tornar-se requisito central.
- Mudança material de equipe, plataformas ou modelo operacional.


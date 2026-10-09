# Aproveitamento do aplicativo atual

## Fonte

Projeto de referência: `../App-Promessams`

Regra absoluta nesta fase: o projeto de referência permanece intacto. O App Promessa é uma reconstrução orientada pelo novo domínio, não uma cópia de diretório.

## Stack observada no projeto atual

- Expo SDK 54.
- React Native 0.81 e React 19.
- Expo Router com exportação web estática.
- TypeScript.
- TanStack Query.
- tRPC/Express no backend atual.
- Drizzle + MySQL.
- AsyncStorage e Secure Store.
- Dependência de Expo Notifications já presente.
- NativeWind/Tailwind.
- Vitest e testes existentes.

Essas informações são inventário, não aprovação automática para o novo projeto.

## Capacidades existentes a avaliar

Rotas e recursos identificados:

- início;
- conteúdos;
- chat;
- ofertas;
- conta;
- administração;
- acessibilidade;
- Bíblia livre;
- hinário;
- pão diário/devocionais;
- plano bíblico;
- política de privacidade;
- autenticação/OAuth atual;
- laboratório de tema.

Conteúdos e ativos relevantes aparentes:

- base do hinário HBJ;
- dados de pão diário;
- plano de leitura;
- conhecimento e documentos do Projeto Sementes;
- materiais de ministérios regionais;
- imagens, ícones e ativos de loja;
- política de privacidade atual;
- testes de acessibilidade, chat, hinário e conteúdo.

## O que provavelmente pode ser reaproveitado

Após revisão de licença, atualização e qualidade:

- conteúdo estático e seus scripts de extração;
- imagens e identidade visual aprovadas;
- componentes acessíveis bem testados;
- casos de teste que expressem comportamento ainda desejado;
- regras de busca do hinário;
- dados e lógica do plano de leitura;
- decisões visuais validadas por usuários.

## O que deve ser redesenhado

- autenticação atual;
- tabela de usuários com papel global;
- backend Express/tRPC como autoridade principal;
- modelo MySQL/Drizzle atual, que não representa organizações, convenções, igrejas, vínculos ou escalas;
- autorização administrativa;
- persistência e sincronização offline;
- pipeline de notificações;
- separação entre dados públicos, privados e sensíveis.

## Processo de migração por capacidade

Para cada recurso:

1. Descrever a necessidade atual, sem olhar primeiro para a implementação antiga.
2. Confirmar que o recurso pertence ao novo escopo.
3. Inventariar arquivos, dados, dependências e testes do legado.
4. Verificar licença, procedência, privacidade e atualização do conteúdo.
5. Escolher entre reutilizar, adaptar ou reimplementar.
6. Definir critérios de aceitação e testes.
7. Migrar em uma alteração pequena e rastreável.
8. Comparar comportamento quando a compatibilidade for necessária.
9. Registrar diferenças intencionais.

## Registro futuro de migrações

Quando a migração começar, adicionar uma tabela por capacidade:

| Capacidade | Origem | Decisão | Dependências | Testes | Estado |
|---|---|---|---|---|---|
| Exemplo: busca no hinário | `shared/hymn-index.ts` | A avaliar | base HBJ | busca e acentos | Não iniciado |

## O que não copiar

- arquivos gerados como `web-dist`;
- segredos ou arquivos de ambiente;
- identificadores EAS, bundle IDs e URLs sem revisão;
- dependências sem uso comprovado;
- código `_core` ligado a plataforma/gerador anterior sem entendimento;
- dados pessoais de desenvolvimento ou produção;
- histórico de autenticação ou tokens.

## Critérios para considerar o legado intacto

- `git status --short` do projeto antigo permanece vazio após operações documentais.
- Nenhuma tarefa do App Promessa altera arquivos no diretório antigo sem pedido explícito.
- Qualquer leitura ou cópia futura identifica claramente o caminho de origem.


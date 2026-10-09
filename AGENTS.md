# Instruções para agentes de IA

Estas instruções se aplicam a todo o projeto App Promessa.

## Antes de trabalhar

1. Leia `README.md` e `docs/README.md`.
2. Leia os documentos relacionados à tarefa.
3. Consulte os ADRs em `docs/decisions/` antes de alterar arquitetura, domínio, autenticação ou segurança.
4. Diferencie explicitamente:
   - fato confirmado pelo responsável do produto;
   - decisão arquitetural vigente;
   - hipótese de trabalho;
   - questão ainda aberta.
5. Não presuma que uma funcionalidade do aplicativo anterior deve ser migrada.

## Relação com o aplicativo atual

- O projeto `../App-Promessams` deve permanecer intacto.
- Trate-o como referência somente leitura até que exista uma tarefa explícita de migração.
- Não copie arquivos, dependências ou padrões arquiteturais em massa.
- Antes de reaproveitar algo, registre origem, finalidade, dependências, riscos e teste de aceitação em `docs/06-aproveitamento-do-legado.md` ou documento de migração mais específico.

## Manutenção de contexto

- Atualize a documentação no mesmo trabalho que alterar comportamento, esquema, contrato ou decisão.
- Toda decisão arquitetural difícil de reverter precisa de um ADR em `docs/decisions/`.
- Não reescreva a história de um ADR aceito. Marque-o como substituído e crie outro.
- Adicione novos documentos ao índice `docs/README.md`.
- Registre dúvidas de produto em `docs/07-roadmap-e-questoes-abertas.md`; não invente regras de negócio silenciosamente.
- Use links relativos entre documentos e caminhos concretos ao citar código.

## Convenções futuras de código

Quando a implementação começar:

- código, tabelas, APIs e identificadores técnicos em inglês;
- textos de interface e documentação de produto em português do Brasil;
- TypeScript estrito, sem `any` não justificado;
- regras de negócio fora de componentes visuais;
- nenhuma tela acessa diretamente credenciais privilegiadas;
- mudanças de banco somente por migrations versionadas;
- toda tabela exposta ao cliente deve possuir RLS e testes de autorização;
- nenhuma chave `service_role` ou segredo pode entrar no cliente, repositório ou logs;
- CPF, tokens e dados de autenticação nunca aparecem em analytics ou logs;
- operações de notificação precisam ser idempotentes;
- funcionalidades administrativas devem produzir auditoria quando alterarem permissões, membros, cultos ou escalas.

## Compatibilidade

- Aplicativos mobile antigos podem continuar instalados por meses.
- Prefira mudanças aditivas: adicionar campo opcional, publicar clientes compatíveis, preencher dados e somente depois endurecer ou remover.
- Não renomeie ou remova contratos em uma única entrega.
- Sempre considere pelo menos a versão atual e a versão anterior do aplicativo.

## Qualidade mínima

Uma mudança só está concluída quando:

- os critérios de aceitação estão atendidos;
- os testes relevantes foram executados;
- autorização e isolamento entre igrejas foram verificados quando aplicáveis;
- estados online, offline, vazio, carregando e erro foram considerados;
- acessibilidade foi verificada;
- documentação e ADRs estão coerentes;
- não houve alteração acidental no projeto `../App-Promessams`.

## Restrições atuais

Neste estágio, o projeto é documental. Não inicialize Expo, Supabase, Git, CI ou qualquer dependência sem uma solicitação explícita.


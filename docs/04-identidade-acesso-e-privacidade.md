# Identidade, acesso e privacidade

## Requisitos confirmados

- Os membros da implantação inicial serão previamente cadastrados por administrador autorizado.
- Para essa organização, o CPF será a forma familiar de localizar/iniciar o acesso.
- Papéis iniciais, como membro ou pastor, já poderão estar atribuídos antes do primeiro acesso.
- Depois de autenticado e vinculado ao cadastro correto, o usuário poderá associar Google e Apple.
- Pastores e responsáveis só podem alterar dados dentro de suas igrejas e escopos autorizados.

## Princípio central

**CPF identifica um cadastro, mas não autentica uma pessoa sozinho.**

O CPF é conhecível, pode vazar de outras fontes e não é segredo. Data de nascimento, últimos dígitos do telefone ou senha padrão também não são provas suficientes.

Detalhes da decisão estão no [ADR-0003](decisions/ADR-0003-cpf-e-vinculo-de-identidades.md).

## Fluxo de ativação proposto

### Pré-cadastro

1. Administrador cria ou importa o membro.
2. Sistema normaliza e protege o CPF.
3. Sistema associa igreja, vínculo e papéis iniciais.
4. O cadastro fica em estado `pending_claim`.
5. Nenhuma senha padrão é criada.

### Primeiro acesso

1. Usuário seleciona “Primeiro acesso”.
2. Informa CPF.
3. Sistema responde de forma que não revele facilmente se o CPF existe.
4. Usuário prova identidade por um método aprovado:
   - código enviado ao telefone/WhatsApp previamente cadastrado; ou
   - código descartável entregue presencialmente por responsável autorizado.
5. Sistema vincula o usuário do provedor de autenticação ao cadastro de pessoa.
6. Sessão é persistida no dispositivo.
7. Aplicativo oferece vínculo opcional com Google/Apple e proteção local por biometria/PIN do aparelho.

O método exato de prova adicional e recuperação ainda precisa de decisão de produto e teste com usuários reais.

## Acessos posteriores

- Priorizar sessão persistente e renovação segura; a melhor simplificação é não pedir login repetidamente.
- Se a sessão for perdida, permitir recuperação por método verificado ou suporte presencial controlado.
- Não permitir que administrador visualize ou defina a senha permanente do usuário.
- Revogar sessões quando houver suspeita, desligamento ou troca sensível de identidade.

## CPF + senha

Supabase Auth aceita nativamente email ou telefone como identificador de senha, não CPF arbitrário. Caso “CPF + senha” seja obrigatório na interface, será necessária uma camada segura no backend que resolva o CPF protegido para a identidade interna, aplique rate limit e delegue a verificação ao provedor de autenticação.

Essa camada:

- não pode armazenar senha;
- não pode expor o email/telefone interno;
- deve usar mensagens genéricas;
- deve registrar apenas metadados seguros de auditoria;
- deve possuir proteção contra força bruta e enumeração.

## Google e Apple

Vínculos sociais são métodos adicionais de entrada na mesma pessoa. Não devem criar automaticamente um novo membro ou conceder papel com base no email.

Fluxo seguro:

1. pessoa ativa o cadastro pré-existente;
2. já autenticada, escolhe vincular Google ou Apple;
3. o identificador imutável do provedor é associado à pessoa;
4. acessos futuros por esse provedor chegam à mesma conta.

### Limite importante sobre aniversário

Não planejar o aniversário como dado obtido automaticamente no login social:

- OpenID Connect básico do Google retorna normalmente identificador, nome, foto e email; aniversário exige People API, escopo adicional e consentimento, e pode não estar disponível.
- Sign in with Apple fornece identificador e, conforme consentimento, nome/email; não fornece aniversário, e o nome só é entregue na primeira autorização.

Portanto, aniversário deve ser solicitado diretamente ao membro, com finalidade clara, ou importado de uma fonte administrativa legítima. Vincular uma conta social não é autorização para coletar dados adicionais desnecessários.

Referências:

- [Google OpenID Connect](https://developers.google.com/identity/openid-connect/reference)
- [Google People API e escopo de aniversário](https://developers.google.com/people/api/rest/v1/people/get)
- [Sign in with Apple](https://developer.apple.com/documentation/signinwithapple/authenticating-users-with-sign-in-with-apple)
- [Supabase identity linking](https://supabase.com/docs/guides/auth/auth-identity-linking)

## Autorização

Autenticação responde “quem é?”. Autorização responde “o que pode fazer neste contexto?”. Não misturar as duas.

### Modelo

- vínculo de membro com igreja;
- atribuição de papel com escopo;
- políticas RLS baseadas em organização, unidade e igreja;
- operações sensíveis em funções/RPCs com validação explícita;
- auditoria para concessão e revogação de acesso.

### Matriz inicial de capacidade

| Capacidade | Visitante | Membro | Responsável | Pastor/admin da igreja | Admin regional/plataforma |
|---|---:|---:|---:|---:|---:|
| Ver evento público | Sim | Sim | Sim | Sim | Sim |
| Ver agenda pessoal | Não | Própria | Própria | Própria | Conforme necessidade |
| Criar rascunho de evento | Não | Não | No escopo | No escopo | No escopo |
| Publicar evento | Não | Não | Se concedido | No escopo | No escopo |
| Alterar escala | Não | Não | Se concedido | No escopo | No escopo |
| Gerenciar membro | Não | Não | Não por padrão | Conforme política | No escopo |
| Conceder papel elevado | Não | Não | Não | Limitado | Conforme política |

A matriz é ponto de partida e precisa de validação funcional.

## Regras mínimas de RLS

- Dados públicos retornam somente registros publicados e campos explicitamente públicos.
- Usuário lê seus próprios dados pessoais.
- Membro não lista dados pessoais de todos os membros por padrão.
- Pastor/admin acessa apenas igrejas cobertas por atribuição ativa.
- Alterar `church_id`, `organization_id` ou papel nunca depende apenas de valor enviado pelo cliente.
- Administrador de organização A nunca acessa organização B.
- Chaves privilegiadas são exclusivas do backend confiável.

## Proteção do CPF

- Normalizar para dígitos somente antes do processamento.
- Validar dígitos verificadores apenas como qualidade de entrada, não como prova de identidade.
- Manter a busca por um HMAC determinístico com segredo fora do banco/cliente.
- Guardar o CPF completo criptografado somente se houver finalidade real que não possa ser atendida pelo HMAC.
- Exibir no máximo uma forma mascarada, por exemplo últimos quatro dígitos, a quem realmente precisa.
- Não incluir em URL, push, analytics, crash report ou log.
- Tabelas de claim/identidade ficam em schema privado, acessíveis somente por funções confiáveis.

## LGPD e natureza sensível

CPF é dado pessoal. Além disso, a associação de uma pessoa a igreja ou organização de caráter religioso pode constituir dado pessoal sensível. O projeto precisa aplicar minimização, finalidade, controle de acesso, retenção, segurança e canal para o titular desde o início.

Antes da produção, definir:

- controlador(es) e operadores;
- hipóteses legais por finalidade;
- política de privacidade;
- retenção e exclusão;
- processo de correção e exportação;
- resposta a incidentes;
- tratamento de dados de menores;
- responsável/canal de privacidade.

Este documento é orientação de produto/engenharia, não parecer jurídico.

Referências:

- [Lei Geral de Proteção de Dados](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm)
- [ANPD: dados pessoais sensíveis](https://www.gov.br/anpd/pt-br/canais_atendimento/cidadao-titular-de-dados/denuncia-peticao-de-titular-referente-lgpd)
- [Guia de segurança para agentes de pequeno porte](https://www.gov.br/anpd/pt-br/centrais-de-conteudo/materiais-educativos-e-publicacoes/anonimizado___guia_orientat-_seg_da_inf_p_atpp.pdf)
- [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)


# Domínio e modelo conceitual

Este documento descreve conceitos, relações e invariantes. Ele não é ainda uma migration nem um esquema físico definitivo.

## Hierarquia institucional

```text
Plataforma
└── País
    └── Organização religiosa / denominação
        └── Unidade administrativa
            └── Igreja local
```

No primeiro cenário:

```text
Brasil
└── Igreja Adventista da Promessa
    └── Convenção Sul-Mato-Grossense
        ├── Igreja A
        ├── Igreja B
        └── demais igrejas locais
```

### Por que usar “unidade administrativa” no núcleo

“Convenção” é o nome correto no contexto inicial. Outras organizações podem usar regional, distrito, diocese, associação ou campo. O núcleo deve possuir uma unidade administrativa com um tipo configurável; a interface inicial pode exibir “Convenção”.

## Limites de dados

- **Organização** é o limite de tenant: dados de organizações diferentes não se misturam.
- **Unidade administrativa** agrupa igrejas e permite administração regional.
- **Igreja local** é o limite operacional mais comum para programação, membros e papéis.
- Uma pessoa é global dentro da plataforma, mas seus vínculos e permissões são contextuais.

## Entidades institucionais

### Country

- `id`
- código ISO, por exemplo `BR`
- nome localizado
- estado ativo

### Organization

Representa a organização religiosa/denominação que adota o produto.

- `id`
- nome legal e nome de exibição
- identificador curto/slug
- configurações de marca
- país de origem opcional
- estado ativo

Não utilizar “Promessa” como filtro implícito no código. Marca, conteúdo e regras específicas devem pertencer à configuração da organização.

### AdministrativeUnit

Representa inicialmente uma convenção.

- `id`
- `organization_id`
- `country_id`
- tipo, inicialmente `convention`
- nome
- código/slug
- unidade-pai opcional para uma futura hierarquia
- estado ativo

Invariante: uma unidade nunca pode apontar para organização diferente de sua unidade-pai.

### Church

- `id`
- `organization_id`
- `administrative_unit_id`
- nome de exibição
- nome legal opcional
- endereço estruturado
- latitude/longitude opcionais
- fuso horário
- canais de contato públicos opcionais
- estado: rascunho, ativa, inativa ou arquivada

Mesmo que `organization_id` possa ser inferido pela unidade, mantê-lo pode facilitar RLS e consultas. Caso seja duplicado fisicamente, uma constraint ou trigger deve impedir inconsistência.

### Address

O endereço deve possuir campos estruturados, não apenas uma string:

- país;
- estado/província;
- cidade;
- bairro;
- logradouro;
- número;
- complemento;
- CEP/código postal;
- texto de exibição opcional.

## Pessoas, cadastro e vínculos

### Person/Profile

Representa a pessoa no produto, separada do método de login.

- `id`
- nome preferido e nome completo
- data de nascimento opcional e sua origem
- foto opcional
- dados de contato autorizados
- preferências
- estado da conta

Uma pessoa não deve ser identificada internamente pelo CPF, email ou ID do Google. O identificador interno deve ser um UUID imutável.

### PreRegistration / MemberClaim

Registro criado pelo administrador antes do primeiro acesso.

- pessoa ou dados mínimos para futura pessoa;
- referência protegida do CPF;
- telefone previamente conferido, se disponível;
- igreja e papel inicial;
- código/estado de ativação;
- data de expiração quando aplicável;
- quem criou e quando.

### ExternalIdentity

Vincula a pessoa aos provedores de autenticação:

- usuário do Supabase Auth;
- telefone;
- Google;
- Apple;
- outros provedores futuros.

Uma identidade externa pertence a uma única pessoa. Uma pessoa pode possuir várias identidades.

### ChurchMembership

Relaciona pessoa e igreja.

- `person_id`
- `church_id`
- tipo de vínculo
- igreja principal ou secundária
- estado: pendente, ativo, suspenso, encerrado
- início e fim
- dados de auditoria

Invariantes iniciais:

- uma pessoa pode ter vários vínculos;
- no máximo um vínculo principal ativo por organização, salvo regra futura explícita;
- remover o vínculo não deve apagar a pessoa nem o histórico de escalas.

### RoleAssignment

Permissão contextual separada do vínculo.

- pessoa;
- papel;
- escopo: organização, unidade administrativa ou igreja;
- escopo concreto;
- validade opcional;
- concedido por;
- revogado por;
- timestamps.

Papéis iniciais possíveis, ainda sujeitos a validação:

- membro;
- responsável por programação;
- líder de ministério;
- secretário;
- pastor;
- administrador de igreja;
- administrador de convenção;
- administrador da plataforma.

Não armazenar apenas `profiles.role = 'pastor'`. A mesma pessoa pode administrar uma igreja e ser apenas visitante em outra.

## Programação e escalas

### EventType

Tipo configurável dentro da organização, por exemplo culto, ensaio, reunião ou congresso.

### Event

Ocorrência concreta com:

- organização, unidade e igreja;
- tipo;
- título e descrição;
- início, fim e fuso horário;
- local/endereço;
- visibilidade;
- estado editorial: rascunho, publicado, cancelado, concluído;
- regra de recorrência ou referência a um modelo, se adotado;
- criador e última alteração.

Visibilidades candidatas:

- pública;
- membros da organização;
- membros da unidade;
- membros da igreja;
- convidados específicos.

### ProgramItem

Item ordenado dentro do evento, quando for necessário detalhar a sequência do culto.

- evento;
- ordem;
- título;
- duração estimada;
- observações internas e públicas separadas.

### Assignment

Escala de uma pessoa para um evento ou item.

- evento/item;
- pessoa;
- função;
- estado: convidada, pendente, confirmada, recusada, substituída ou cancelada;
- observações autorizadas;
- pessoa que realizou a escala;
- timestamps.

Invariantes:

- a pessoa precisa estar elegível no contexto definido pela organização;
- uma alteração de pessoa, função, horário ou cancelamento gera evento de domínio/auditoria;
- notificações não devem ser enviadas dentro da transação principal; devem sair de uma fila/outbox idempotente.

### Ministry e AssignmentFunction

Funções como pregação, vocal, instrumento, recepção ou direção devem ser configuráveis. Não usar enum rígido enquanto o vocabulário ainda estiver mudando.

## Comunicação e dispositivo

### DeviceInstallation

- pessoa;
- instalação/dispositivo;
- plataforma;
- push token atual;
- versão do aplicativo;
- última atividade;
- estado da permissão de notificação;
- data de invalidação.

Um usuário pode possuir vários dispositivos e um token pode mudar.

### NotificationOutbox

- tipo de mensagem;
- destinatário;
- referência do evento/escala;
- payload mínimo;
- chave de idempotência;
- agendamento;
- tentativas e último erro;
- estado de entrega.

### AuditLog

Deve registrar pelo menos:

- ator;
- ação;
- recurso e escopo;
- antes/depois quando apropriado e seguro;
- timestamp;
- origem técnica;
- correlação da requisição.

Não registrar CPF, senha, token, código OTP ou segredo no log.

## Estratégia para campos em evolução

- Normalizar o núcleo: organizações, igrejas, pessoas, vínculos, eventos e escalas.
- Usar tabelas configuráveis para papéis, tipos e funções que mudam com frequência.
- Usar `jsonb` somente para metadados acessórios e versionados.
- Preferir UUIDs e timestamps com fuso.
- Adotar exclusão lógica/arquivamento onde o histórico importa.
- Alterações devem ser aditivas e compatíveis com clientes antigos.

## Diagrama conceitual resumido

```text
Country ──< AdministrativeUnit >── Organization
                    │                    │
                    └──< Church >────────┘
                           │
Person ──< ChurchMembership┤
   │                       │
   ├──< ExternalIdentity   └──< Event ──< ProgramItem
   ├──< RoleAssignment              └──< Assignment >── Person
   └──< DeviceInstallation

Assignment/Event ──< NotificationOutbox
Actor/Resource ────< AuditLog
```

## Pontos ainda não definidos

- Uma igreja pode pertencer simultaneamente a mais de uma unidade?
- Convenções podem atravessar países?
- Haverá congregações, pontos de pregação ou grupos abaixo de igreja?
- Quais informações da pessoa podem aparecer publicamente em uma escala?
- Menores de idade utilizarão contas próprias?
- Quais papéis podem cadastrar, remover ou transferir membros?
- Como igrejas vizinhas serão determinadas: mesma unidade, seleção manual ou distância geográfica?


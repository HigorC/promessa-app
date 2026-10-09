# Roadmap e questões abertas

## Estado atual: fase documental

Objetivo desta fase:

- consolidar visão e domínio;
- registrar decisões arquiteturais;
- separar fatos de hipóteses;
- preparar um projeto fácil de continuar por pessoas e agentes de IA;
- não inicializar código antes de definir o primeiro recorte de produto.

## Roadmap sugerido

### Fase 0 — Descoberta e validação

- Validar vocabulário institucional.
- Obter lista real de igrejas, papéis e tipos de programação.
- Entrevistar representantes de membros com diferentes níveis de familiaridade digital.
- Desenhar protótipo do primeiro acesso e teste presencial.
- Definir política de privacidade e responsabilidades LGPD.
- Confirmar dados públicos versus internos.
- Definir critérios do MVP.

### Fase 1 — Fundação técnica

- Inicializar Expo/TypeScript e Supabase local.
- Criar design system acessível básico.
- Configurar CI e ambientes.
- Implementar esquema institucional e RLS.
- Adicionar observabilidade sem dados sensíveis.
- Criar testes de isolamento multi-tenant.

### Fase 2 — Identidade e membros

- Importação/pré-cadastro de membros.
- Ativação baseada em CPF com prova adicional.
- Sessão persistente e recuperação.
- Vínculos por igreja.
- Papéis por escopo e auditoria.
- Vínculo opcional Google/Apple.

### Fase 3 — Igrejas, eventos e escalas

- Consulta pública de igrejas e agenda.
- Gestão de eventos em rascunho e publicação.
- Programação detalhada.
- Escalas e agenda pessoal.
- Fluxo de alteração/cancelamento.

### Fase 4 — Offline e notificações

- Cache SQLite e sincronização incremental.
- Registro de dispositivos.
- Outbox e push de escalas.
- Lembretes e deep links.
- Métricas e tratamento de tokens inválidos.

### Fase 5 — Migração seletiva de conteúdo

- Avaliar hinário, Bíblia, devocionais e plano de leitura.
- Migrar somente capacidades aprovadas.
- Revisar conteúdo, licenças, acessibilidade e armazenamento.

### Fase 6 — Expansão

- Outras convenções.
- Outras organizações religiosas.
- Personalização de marca e vocabulário.
- Novas estruturas administrativas e países.

## Recorte de MVP sugerido

O MVP deve provar três jornadas:

1. Um visitante encontra uma igreja e consulta sua programação publicada.
2. Um membro pré-cadastrado ativa a conta, vê a própria agenda e continua consultando-a offline.
3. Um responsável cria um culto, escala uma pessoa e essa pessoa recebe/visualiza a alteração.

Todo recurso que não fortalece uma dessas jornadas deve justificar sua entrada no MVP.

## Questões que bloqueiam decisões de implementação

### Organização

- Qual é o nome oficial completo e a sigla da organização e da convenção inicial?
- Quais são as sete igrejas, seus endereços e fusos?
- Existem congregações abaixo de uma igreja local?
- Quem poderá criar uma nova igreja?

### Pessoas e acesso

- Todos possuem CPF e telefone próprio?
- Qual canal é mais acessível: SMS, WhatsApp ou código presencial?
- Quem pode corrigir CPF/telefone e como essa alteração será confirmada?
- Haverá menores com conta própria?
- Por quanto tempo um cadastro pendente permanece válido?
- Pastor precisa usar MFA obrigatório?

### Papéis

- Quais cargos reais existem hoje?
- Um pastor administra uma ou várias igrejas?
- Líderes de ministério podem editar somente funções do próprio ministério?
- Quem concede e revoga papéis elevados?

### Programação

- Quais tipos de culto/evento existem?
- Programação significa apenas data/horário ou também ordem completa do culto?
- Eventos são recorrentes? Como exceções e feriados funcionam?
- A pessoa precisa aceitar/recusar uma escala?
- Quanto tempo antes enviar lembretes?
- Quais dados do escalado aparecem publicamente?

### Igrejas vizinhas

- Vizinha significa mesma convenção, seleção manual ou distância?
- O usuário seguirá igrejas favoritas?
- Há eventos regionais sem uma igreja organizadora única?

### Privacidade e dados

- Para que o aniversário será usado?
- Data completa é necessária ou apenas dia e mês?
- Qual é a base e o período de retenção para cada dado?
- Quem responde às solicitações de titulares?
- Quais relatórios administrativos serão necessários?

### Legado

- Quais recursos atuais são indispensáveis no primeiro lançamento?
- Existem licenças ou autorizações formais para hinário, Bíblia e demais documentos?
- O nome público continuará PromessaMS ou será App Promessa?
- Quais métricas/feedback do aplicativo atual estão disponíveis?

## Riscos conhecidos

- Tentar migrar todas as funções antigas antes de validar o núcleo de igreja/agenda.
- Usar CPF como segredo ou deixar CPF acessível ao cliente.
- Confundir papel global com permissão por igreja.
- Confiar em Google/Apple para fornecer aniversário.
- Criar abstração religiosa excessivamente genérica antes do primeiro caso real.
- Projetar edição offline complexa sem necessidade comprovada.
- Enviar múltiplas notificações para a mesma mudança.
- Quebrar versões antigas do app com migrations destrutivas.
- Expor filiação religiosa ou escalas pessoais como conteúdo público.

## Próximo passo recomendado

Realizar uma sessão de descoberta focada em:

1. estrutura real da convenção e das igrejas;
2. lista de papéis e poderes;
3. anatomia de um culto e de uma escala;
4. jornada de primeiro acesso para três perfis de usuário;
5. definição do que é público.

Com essas respostas, produzir fluxos, matriz de autorização validada e primeiro esquema lógico antes de inicializar código.


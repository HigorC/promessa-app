# Contexto e visão do produto

## Visão

O App Promessa será uma central digital para a vida cotidiana das igrejas. Ele deve reunir programação de cultos e eventos, escalas de participação, conteúdo institucional e comunicação relevante, oferecendo uma experiência simples para membros e ferramentas seguras para a liderança.

O produto começa na Igreja Adventista da Promessa, dentro da Convenção Sul-Mato-Grossense, mas não deve codificar nomes, regras visuais ou estruturas de dados que tornem impossível atender outra convenção, outro país ou outra organização religiosa.

## Cenário inicial confirmado

- Aproximadamente sete igrejas.
- Em torno de cinquenta membros por igreja, cerca de 350 pessoas no início.
- Cada membro possui uma igreja local principal.
- O membro vê prioritariamente a programação de sua igreja.
- O membro pode consultar programações publicadas de igrejas vizinhas.
- A liderança cadastra cultos, eventos, programação e escalas.
- Quando uma pessoa for escalada, por exemplo para cantar ou pregar, ela deve poder receber notificação.
- Parte do conteúdo será pública e utilizável sem login.
- Recursos pessoais ou administrativos exigirão autenticação.
- Conteúdo já sincronizado deverá continuar útil quando o dispositivo estiver offline.
- Android, iOS e web são plataformas de primeira classe.

## Estrutura institucional confirmada

Para o primeiro uso:

1. País: Brasil.
2. Organização/denominação: Igreja Adventista da Promessa.
3. Convenção: Convenção Sul-Mato-Grossense.
4. Igrejas locais vinculadas à convenção.

Somente a convenção inicial precisa operar no primeiro lançamento, mas país, organização e unidade administrativa não podem ser valores fixos no código.

## Públicos principais

### Visitante

- Não possui conta ou não está autenticado.
- Consulta igrejas, endereços e programações explicitamente publicadas.
- Não vê dados pessoais, rascunhos, telefones ou informações internas.

### Membro

- Foi previamente cadastrado por um administrador autorizado.
- Possui uma igreja local e pode ter vínculos adicionais.
- Consulta sua programação, escalas e conteúdos permitidos.
- Pode acompanhar igrejas vizinhas.
- Recebe notificações relacionadas às suas escalas e preferências.

### Responsável por programação

- Pode ser secretário, líder de ministério, diretor ou outro papel configurado.
- Cria ou edita eventos e escalas dentro de um escopo determinado.
- Não recebe automaticamente permissão para gerenciar membros ou segurança.

### Pastor ou administrador de igreja

- Gerencia dados e atividades de uma ou mais igrejas explicitamente atribuídas.
- Pode cadastrar ou ajustar membros conforme a política futura.
- Não pode administrar outra igreja apenas por possuir o título de pastor em seu perfil.

### Administrador de convenção/plataforma

- Administra o escopo atribuído da convenção ou da plataforma.
- Executa importações, cria igrejas e concede papéis elevados.
- Toda ação sensível deve ser auditável.

## Problemas que o produto resolve

- Programações espalhadas em mensagens e arquivos diferentes.
- Dificuldade de um membro saber quando e onde participará.
- Falta de um canal confiável para alterações de escala.
- Dependência de redes sociais ou grupos para informações básicas.
- Dificuldade da liderança em manter uma visão consolidada por igreja.
- Barreiras de acesso para pessoas com pouca familiaridade tecnológica.

## Princípios de experiência

- O usuário não deve ser obrigado a entrar para ver conteúdo realmente público.
- O primeiro acesso deve usar linguagem direta, botões grandes e poucos passos.
- A sessão deve permanecer ativa com segurança; não se deve exigir login a cada abertura.
- “Minha igreja” deve ser a visão inicial do membro.
- Trocar para uma igreja vizinha deve ser simples, mas nunca confundir o escopo administrativo.
- Informações críticas devem indicar quando foram atualizadas pela última vez.
- A experiência deve funcionar com fonte ampliada, leitores de tela e contraste adequado.

## Escopo funcional inicial proposto

### Fundação

- Países, organizações, convenções/unidades administrativas e igrejas.
- Cadastro prévio de membros.
- Identidade e ativação por CPF com prova adicional de posse/identidade.
- Papéis e permissões por igreja.

### Programação

- Tipos de evento, por exemplo culto, reunião, ensaio e programação especial.
- Ocorrências com data, hora, local, visibilidade e estado de publicação.
- Itens de programação e escalas com função e pessoa responsável.
- Alteração e cancelamento com histórico mínimo.

### Consulta e comunicação

- Agenda pública por igreja.
- Agenda personalizada do membro.
- Notificações de nova escala, alteração, cancelamento e lembrete.
- Cache local para leitura offline.

### Administração

- Painel responsivo para gestão de igreja.
- Auditoria de mudanças sensíveis.
- Importação inicial de membros por formato a definir.

## Fora do primeiro MVP

Até decisão posterior, não assumir no MVP:

- contabilidade completa;
- doações ou pagamentos;
- transmissão de cultos;
- chat em tempo real;
- rede social interna;
- sincronização offline de edições administrativas;
- gestão completa de múltiplas denominações em produção;
- inferência automática de dados pessoais por redes sociais.

Esses itens podem ser reavaliados, inclusive porque o aplicativo anterior possui recursos de conteúdo, chat e ofertas.

## Métricas iniciais sugeridas

- Percentual de membros que concluem o primeiro acesso.
- Percentual de usuários que permitem notificações.
- Número de escalas visualizadas/confirmadas.
- Tempo necessário para um responsável publicar um evento.
- Quantidade de conflitos ou correções em escalas.
- Uso de leitura offline.
- Chamados de suporte por login.


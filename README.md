# App Promessa

Central digital para igrejas, criada inicialmente para a Igreja Adventista da Promessa e para a Convenção Sul-Mato-Grossense, mas com domínio e arquitetura preparados para outras convenções, países e organizações religiosas.

## Estado atual

Este diretório contém **somente documentação de produto e arquitetura**. Nenhum aplicativo, banco de dados ou infraestrutura foi inicializado.

O aplicativo atual, localizado em `../App-Promessams`, é apenas a fonte de funcionalidades, conteúdo e aprendizados que poderão ser reaproveitados no futuro. Ele deve permanecer intacto. A migração será seletiva: nada será copiado automaticamente.

## Leitura obrigatória para pessoas e agentes de IA

1. [AGENTS.md](AGENTS.md) — regras de trabalho e manutenção do contexto.
2. [Índice da documentação](docs/README.md) — mapa e status dos documentos.
3. [Contexto e visão do produto](docs/01-contexto-e-visao.md).
4. [Domínio e modelo conceitual](docs/02-dominio-e-dados.md).
5. [Arquitetura proposta](docs/03-arquitetura.md).
6. [Autenticação, autorização e privacidade](docs/04-identidade-acesso-e-privacidade.md).
7. [Offline e notificações](docs/05-offline-e-notificacoes.md).
8. [Aproveitamento do aplicativo atual](docs/06-aproveitamento-do-legado.md).
9. [Roadmap e questões abertas](docs/07-roadmap-e-questoes-abertas.md).

## Resumo executivo

- Um usuário poderá consultar programações públicas sem autenticação.
- Membros autenticados acompanharão principalmente sua igreja local e, opcionalmente, igrejas vizinhas.
- Pastores e outros responsáveis administrarão somente as igrejas em que possuírem autorização.
- Pessoas escaladas para cantar, pregar ou exercer outra função poderão receber notificações.
- A primeira implantação deve atender cerca de sete igrejas, com aproximadamente cinquenta membros por igreja.
- A hierarquia inicial é: país → organização/denominação → convenção/unidade administrativa → igreja local.
- Para a implantação inicial, o CPF será o identificador de entrada mais simples para os membros, mas nunca será usado sozinho como prova de identidade.
- Após a ativação, o usuário poderá vincular identidades Google e Apple à mesma conta.
- A direção técnica atual é Expo/React Native no cliente e Supabase no backend.

## Princípios do projeto

1. **Acessível antes de sofisticado.** O público inclui pessoas com pouca familiaridade digital.
2. **Público por padrão apenas quando seguro.** Programações publicadas podem ser abertas; dados pessoais e administrativos não.
3. **Permissão pertence ao contexto.** Ser pastor ou administrador é uma relação com uma igreja ou unidade, não um atributo global da pessoa.
4. **Offline útil.** Conteúdo já sincronizado deve continuar disponível sem internet.
5. **Evolução segura.** Alterações de banco e contratos devem suportar clientes mobile desatualizados.
6. **Genérico sem abstração prematura.** O primeiro caso real guia o produto, mas nomes e limites do domínio não devem impedir outras organizações religiosas.
7. **AI First.** Toda decisão relevante precisa ser fácil de localizar, verificar e atualizar.


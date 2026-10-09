# ADR-0003 — CPF e vínculo de identidades

- Estado: aceita para planejamento
- Data: 2026-10-05

## Contexto

Muitos membros possuem pouca familiaridade com login Google ou criação de contas. Os membros serão pré-cadastrados e reconhecem o CPF como identificador. Também se deseja permitir Google e Apple após o primeiro acesso.

## Decisão

- Apresentar CPF como identificador familiar no fluxo da organização inicial.
- Nunca aceitar CPF sozinho como prova de identidade.
- Criar pré-cadastro sem senha padrão.
- Exigir prova adicional no claim, como OTP em canal previamente verificado ou código presencial descartável.
- Separar pessoa de identidade de autenticação.
- Permitir várias identidades vinculadas à mesma pessoa.
- Conceder papéis por vínculo/escopo, nunca com base no provedor social.
- Proteger a consulta de CPF em backend privado.

## Justificativa

- CPF é fácil para o membro lembrar, mas não é secreto.
- Pré-cadastro permite que permissões sejam preparadas sem abrir autoinscrição pública.
- Vínculo posterior com Google/Apple reduz atrito futuro.
- Separar identidade evita duplicar pessoas quando mudam telefone ou provedor.

## Consequências

- Supabase não oferece CPF como username nativo; uma camada segura poderá ser necessária.
- Será necessário escolher e custear um canal de OTP ou operar códigos presenciais.
- Recuperação de conta e alteração de telefone exigem processo administrativo bem definido.
- CPF e filiação religiosa elevam os requisitos de privacidade e segurança.
- Google/Apple não são fontes confiáveis para aniversário; esse dado terá fluxo próprio.

## Alternativas rejeitadas

- CPF sem segundo fator: permite personificação.
- CPF + data de nascimento: ambos podem ser conhecidos.
- Senha inicial igual ou derivada do CPF: previsível e insegura.
- Conta Google obrigatória: cria barreira para parte do público.
- Criar automaticamente qualquer usuário social como membro: contorna o pré-cadastro e as permissões.

## Pontos pendentes

- Canal principal: WhatsApp, SMS ou outro.
- Fallback para membro sem telefone individual.
- Política de MFA para pastores e administradores.
- Necessidade real de CPF + senha recorrente versus sessão persistente/OTP.


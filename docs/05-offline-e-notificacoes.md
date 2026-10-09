# Offline e notificações

## Objetivo offline

O aplicativo não precisa ser integralmente offline-first no primeiro lançamento. Ele precisa continuar útil quando a conexão falha.

### Deve funcionar offline no MVP

- abrir programações já sincronizadas;
- consultar detalhes já armazenados de eventos;
- consultar as próprias escalas já sincronizadas;
- abrir conteúdos selecionados para cache;
- mostrar data/hora da última sincronização;
- navegar sem travamentos ou telas em branco.

### Pode exigir internet no MVP

- primeiro acesso e recuperação de conta;
- alteração de dados pessoais;
- criação/publicação de eventos;
- mudança de escalas;
- gestão de membros e permissões;
- vínculo Google/Apple;
- confirmação cuja consistência imediata seja obrigatória.

Edição administrativa offline só deve ser implementada quando houver necessidade comprovada, porque introduz fila de escrita, conflitos e reconciliação.

## Estratégia de cache proposta

- TanStack Query controla busca, invalidação e estado remoto.
- SQLite armazena projeções locais importantes.
- Cada conjunto registra versão/`updated_at` e última sincronização.
- Conteúdo público e agenda pessoal têm políticas de retenção próprias.
- Logout remove ou torna inacessível todo cache privado da pessoa.
- Troca de usuário no mesmo dispositivo não pode revelar cache anterior.
- O aplicativo diferencia “sem eventos” de “não foi possível atualizar”.

## Sincronização

Fluxo inicial:

1. Renderizar cache local imediatamente.
2. Se houver rede, buscar alterações desde o cursor/timestamp confiável.
3. Aplicar atualização transacional no cache.
4. Invalidar itens removidos/cancelados.
5. Atualizar indicador de sincronização.

Não confiar apenas no relógio do dispositivo. Cursores devem ser emitidos pelo backend ou baseados em versões/timestamps do servidor.

## Casos de notificação

- pessoa adicionada a uma escala;
- função, data, hora ou local da escala alterados;
- escala ou evento cancelado;
- lembrete antes do evento;
- pedido de confirmação, se essa função for aprovada;
- aviso importante da igreja, sujeito a política e preferências.

Evitar notificações para alterações editoriais irrelevantes.

## Pipeline proposto

```text
Alteração confirmada no banco
        │
Evento gravado na NotificationOutbox
        │
Worker/Edge Function seleciona destinatários
        │
Expo Push Service → FCM/APNs
        │
Resultado atualiza tentativa/token
```

Requisitos:

- chave de idempotência por tipo, recurso, versão e destinatário;
- retry com backoff;
- invalidação de tokens não registrados;
- múltiplos dispositivos por usuário;
- deep link para evento/escala;
- payload sem CPF ou conteúdo pessoal desnecessário;
- envio desacoplado da transação que salva a escala;
- métricas de solicitado, aceito pelo serviço, erro e interação quando disponível.

## Agendamento e lembretes

- Uma rotina periódica identifica lembretes devidos.
- Grava itens idempotentes na outbox.
- O horário considera o fuso da igreja/evento.
- Alteração ou cancelamento invalida lembretes antigos.
- Preferências permitem reduzir avisos não essenciais.

## Limites de garantia

Push é melhor esforço. O sistema operacional, permissão do usuário, bateria, rede ou token inválido podem impedir a entrega. Informações importantes também devem permanecer visíveis dentro do aplicativo.

## Permissão de notificação

- Pedir permissão depois de explicar o benefício concreto.
- Preferir o momento após ativação ou primeira escala, não a primeira tela do app.
- Se negada, mostrar como habilitar posteriormente sem bloquear o uso.
- Registrar a preferência no servidor separadamente do estado técnico do dispositivo.

## Testes essenciais

- Android e iOS em dispositivo físico.
- Aplicativo aberto, em segundo plano e encerrado.
- Token renovado e token inválido.
- Usuário com dois dispositivos.
- Mudança rápida de escala gerando apenas mensagens válidas.
- Evento cancelado depois de lembrete agendado.
- Fuso diferente do dispositivo.
- Logout e troca de usuário.
- Sem rede, rede instável e recuperação da conexão.

## Referências

- [Expo push setup](https://docs.expo.dev/push-notifications/push-notifications-setup/)
- [Expo Notifications API](https://docs.expo.dev/versions/latest/sdk/notifications/)
- [Expo SQLite](https://docs.expo.dev/versions/latest/sdk/sqlite/)
- [Supabase Edge Function para push com Expo](https://supabase.com/docs/guides/functions/examples/push-notifications)


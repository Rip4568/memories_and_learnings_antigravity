# 10/03/2026 - WhatsApp Tracking via Quoted Messages

**Situation/Bug**:
Ao lidar com confirmações ou reagendamentos no WhatsApp via Baileys (respostas simples "1", "2" ou "3"), o bot tentava "adivinhar" qual agendamento alterar buscando o próximo agendamento futuro no banco de dados. Isso pode falhar se:

- O paciente tem múltiplos agendamentos futuros não muito espaçados
- O paciente responde à mensagem de ontem quando tem um agendamento novo
- Outras lógicas/NLP tornam a dedução complexa.

**Solution/Insight**:

1. **Embutir o ID do Agendamento**: Incluir explicitamente o `appointmentId` ou um UUID de rastreio nas mensagens de notificação disparadas ativamente (ex: "*Cod de agendamento: 1234-uuid*").
2. **Utilizar `quotedMessage` no Baileys**: Quando o cliente responde à mesma conversa, a API Baileys traz o contexto original na propriedade `msg.message.extendedTextMessage.contextInfo.quotedMessage.conversation` ou `...quotedMessage.extendedTextMessage.text`.
3. **Regex Seguro**: Passar um regex (ex: `/Cod de agendamento: ([\w-]+)/`) sobre a mensagem citada permite extrair e rastrear 100% de exatidão qual agendamento está sendo modificado, sem necessidade de consultas adivinhadoras ou inteligência artificial complexa.
4. **Resiliência de Arquitetura**: Usar `Enqueuer` / filas do Redis quando o `Baileys` recebe uma mensagem previne que buscas em banco de dados (`AppointmentRepository`) travem o Event-Loop da API do Whatsapp durante picos de mensagens, enquanto possibilita tolerância a falhas caso o DB caia momentaneamente.

**Reference**:

- Implementado no Bio-Organize Backend em `WhatsappService`, `WhatsappReminderSchedule` e `AppointmentService`
- Refere-se à arquitetura em `ProcessWhatsappResponseJob`.

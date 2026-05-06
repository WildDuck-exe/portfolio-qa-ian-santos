# Funcionalidades — Klipper

## 1. Módulo de Autenticação

### 1.1 Login
- Autenticação via username/senha
- Geração de token JWT (validade 24h)
- Armazenamento seguro do token (SharedPreferences)
- Restauração automática de sessão

### 1.2 Registro de Push Token
- Registro de dispositivo para notificações FCM
- Atualização de token quando necessário

### 1.3 Logout
- **NÃO IMPLEMENTADO no backend** — gap documentado
- Client-side apenas (limpa SharedPreferences)
- Token JWT permanece válido por até 24h

---

## 2. Módulo de Clientes

### 2.1 Cadastro de Cliente
- Nome (obrigatório)
- Telefone (único, obrigatório)
- Data de cadastro (automática)

### 2.2 Busca de Cliente
- Por telefone (endpoint público)
- Listagem completa

### 2.3 Gestão (Backend)
- CRUD completo via API
- Validação de telefone formatado

---

## 3. Módulo de Serviços

### 3.1 Cadastro de Serviço
- Nome (obrigatório)
- Descrição
- Duração em minutos (padrão 30)
- Preço
- Categoria (padrão "Geral")
- Status ativo/inativo

### 3.2 Listagem
- Serviços ativos (endpoint público)
- Todos os serviços (admin)
- Filtragem por categoria

### 3.3 Servicios Padrão

| Serviço | Descrição | Duração | Preço | Categoria |
|---------|-----------|---------|-------|----------|
| Corte Social | Corte clássico tesoura e máquina | 30min | R$45 | Cabelo |
| Degradê / Fade | Corte moderno com acabamento na navalha | 45min | R$55 | Cabelo |
| Barba Completa | Barba com toalha quente e massagem | 30min | R$35 | Barba |
| Combo Klipper | Corte + Barba + Lavagem | 60min | R$80 | Combo |

---

## 4. Módulo de Agendamentos

### 4.1 Criação de Agendamento
- Seleção de cliente
- Seleção de serviço
- Data e horário
- Status: `agendado`, `concluido`, `cancelado`
- Observações (opcional)

### 4.2 Cálculo de Horários Disponíveis
- Baseado em `horario_inicio` e `horario_fim`
- Considera pausa do meio-dia (`pausa_inicio` a `pausa_fim`)
- Considera duração do serviço
- Considera dias de trabalho (`dias_trabalho`)

### 4.3 Validações
- Horário dentro do expediente
- Não conflita com pausa
- Não conflita com agendamentos existentes

### 4.4 Fluxo Público (Chat Web)
1. Cliente acessa ChatWeb via link/WhatsApp
2. Informa nome e telefone
3. Seleciona serviço
4. Seleciona data e horário disponível
5. Confirma agendamento
6. Barbeiro recebe notificação push

---

## 5. Módulo de Agenda (App Flutter)

### 5.1 Dashboard
- Cards de resumo do dia
- Pendentes Hoje
- Receita Hoje

### 5.2 Agenda
- Visualização de agendamentos
- Calendário
- Lista de pendentes

### 5.3 Navegação Magic Bottom Nav
```
[0] Dashboard  — dashboard_outlined
[1] Clientes   — people_outline
[2] Agenda     — calendar_today_outlined
[3] Serviços   — content_cut_outlined
[4] Vendas     — monetization_on_outlined
```

---

## 6. Módulo de Vendas/Despesas

### 6.1 Registro de Despesa
- Descrição
- Valor
- Data
- Categoria

### 6.2 Listagem de Despesas
- Visualização por período
- Total por categoria

---

## 7. Módulo de Configurações

### 7.1 Configurações de Horário
| Chave | Padrão | Descrição |
|-------|--------|-----------|
| `horario_inicio` | `08:00` | Início do expediente |
| `horario_fim` | `18:00` | Fim do expediente |
| `dias_trabalho` | `1,2,3,4,5,6` | Dias ativos (0=Dom) |
| `pausa_inicio` | `12:00` | Início da pausa |
| `pausa_fim` | `13:00` | Fim da pausa |

### 7.2 Configurações de WhatsApp
| Chave | Uso |
|-------|-----|
| `whatsapp_mensagem` | Mensagem de boas-vindas |
| `whatsapp_mensagem_pausa` | Mensagem durante pausa |
| `whatsapp_mensagem_fechado` | Mensagem fora do expediente |
| `whatsapp_mensagem_cancelamento` | Mensagem de cancelamento |

---

## 8. Notificações Push

### 8.1 Firebase Cloud Messaging
- Envio de notificações para app Flutter
- Suporte a múltiplos dispositivos
- Dados do agendamento na notificação

### 8.2 Template de Mensagem
```
💈 {nome_cliente} agendou {servico} às {horario}
```

---

## 9. Chat Web (Interface do Cliente)

### 9.1 Fluxo de Agendamento
1. Cliente acessa via link
2. Informa nome
3. Informa telefone
4. Seleciona serviço
5. Seleciona data
6. Seleciona horário disponível
7. Confirma

### 9.2 Validações
- Telefone formatado (máscara)
- Horário disponível (não conflita)
- Campos obrigatórios

### 9.3 Problemas Conhecidos
- `formatarTelefone()` com índices errados
- `showTimes()` com horários hardcodados
- `supabase-config.js` vazio

---

## 10. Bot WhatsApp (Baileys)

### 10.1 Fluxo
1. Cliente envia mensagem para WhatsApp do barbeiro
2. Bot detecta mensagem
3. Busca config do Flask (`/api/public/config`)
4. Responde automaticamente com link do ChatWeb

### 10.2 Status
- **NÃO IMPLEMENTADO** — feature nova

---

## 11. Serviços do App Flutter

### 11.1 api_service.dart
- `login(username, password)`
- `logout()`
- `loadToken()`
- `registerPushToken(token)`
- `getClientes()`
- `getServicos()`
- `getAgendamentos()`
- `createAgendamento(data)`

### 11.2 Firebase
- Inicialização
- Recebimento de tokens FCM
- Tratamento de mensagens em foreground

---

## 12. Segurança

### 12.1 Autenticação
- JWT com expiração 24h
- Senhas hasheadas (não especificado o algoritmo)

### 12.2 CORS
- Configurado no backend Flask

### 12.3 Row Level Security (Supabase)
- Políticas de acesso público para demo
- Tabelas: `clientes`, `servicos`, `agendamentos`

### 12.4 Gaps de Segurança
| Gap | Gravidade | Descrição |
|-----|-----------|-----------|
| Sem endpoint logout | 🔴 CRÍTICO | Token permanece válido após logout client-side |
| Sem email no Usuario | 🟡 MÉDIO | Impossibilita recuperação de senha real |
| Sem validação de input | 🟡 MÉDIO | Potencial para injeção SQL (a verificar) |

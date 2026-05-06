# Bugs Report — Klipper

## Resumo Executivo

| Métrica | Valor |
|---------|-------|
| **Total de Bugs** | 9 |
| **Alta Severidade** | 5 |
| **Média Severidade** | 4 |
| **Crítica** | 0 |

---

## Lista de Bugs

### BUG-001 — formatarTelefone() com índices errados

**Módulo:** Chat Web (chat.js)  
**Severidade:** Alta  
**Prioridade:** Alta  
**Status:** Aberto

**Descrição:**  
A função `formatarTelefone()` contém índices incorretos que fazem a máscara de telefone gerar um formato inválido: `(71)9-2887-024` em vez do formato correto.

**Passos para Reprodução:**
1. Acessar ChatWeb
2. Inserir um número de telefone
3. Observar a formatação gerada

**Resultado Esperado:** Telefone formatado corretamente: `(71) 99999-8888`

**Resultado Obtido:** `(71)9-2887-024` (formato incorreto)

**Arquivo:** `barbearia-backend/static/chat/chat.js`

**Correção Proposta:**
Revisar os índices da função `formatarTelefone()` e corrigir a lógica de formatação.

---

### BUG-002 — showTimes() com horários hardcodados

**Módulo:** Chat Web (chat.js)  
**Severidade:** Alta  
**Prioridade:** Alta  
**Status:** Aberto

**Descrição:**  
A função `showTimes()` retorna horários hardcodados, ignorando a configuração de pausa, duração do serviço e dias de trabalho definidos no sistema.

**Passos para Reprodução:**
1. Acessar ChatWeb
2. Selecionar um serviço
3. Observar os horários disponíveis

**Resultado Esperado:** Horários calculados dinamicamente baseados em:
- `horario_inicio` / `horario_fim`
- `pausa_inicio` / `pausa_fim`
- Duração do serviço selecionado
- Agendamentos existentes

**Resultado Obtido:** Horários fixos/hardcodados, independendo da configuração

**Arquivo:** `barbearia-backend/static/chat/chat.js`

---

### BUG-003 — supabase-config.js vazio

**Módulo:** Chat Web  
**Severidade:** Alta  
**Prioridade:** Alta  
**Status:** Aberto

**Descrição:**  
O arquivo `static/chat/supabase-config.js` está vazio, fazendo com que o ChatWeb servido pelo Flask não funcione corretamente. O ChatWeb precisa das credenciais do Supabase para inserir dados.

**Passos para Reprodução:**
1. Acessar ChatWeb via Flask
2. Tentar criar um agendamento

**Resultado Esperado:** ChatWeb funcional, inserindo dados no Supabase

**Resultado Obtido:** ChatWeb não funciona por falta de configuração do Supabase

**Arquivo:** `barbearia-backend/static/chat/supabase-config.js`

---

### BUG-004 — notifications.py crash na 2ª chamada

**Módulo:** Backend (notifications.py)  
**Severidade:** Média  
**Prioridade:** Alta  
**Status:** Aberto

**Descrição:**  
A função de notificação não verifica se `firebase_admin._apps` já está inicializado, causando crash ao tentar enviar notificações pela segunda vez.

**Passos para Reprodução:**
1. Enviar uma notificação push (primeira chamada)
2. Tentar enviar outra notificação

**Resultado Esperado:** Notificação enviada com sucesso

**Resultado Obtido:** Crash na segunda chamada

**Arquivo:** `barbearia-backend/utils/notifications.py`

**Correção Proposta:**
```python
if not firebase_admin._apps:
    firebase_admin.initialize_app(cred)
```

---

### BUG-005 — send_multicast() depreciado

**Módulo:** Backend (notifications.py)  
**Severidade:** Média  
**Prioridade:** Média  
**Status:** Aberto

**Descrição:**  
O código usa `send_multicast()` que está depreciado na versão atual do Firebase Admin SDK.

**Resultado Esperado:** Usar método não depreciado

**Resultado Obtido:** Código funciona mas com warning de deprecação

**Arquivo:** `barbearia-backend/utils/notifications.py`

**Correção Proposta:**  
Migrar para `send_each` ou `send_all` conforme documentação do Firebase.

---

### BUG-006 — baseUrl hardcoded para emulador

**Módulo:** App Flutter (main.dart / api_service.dart)  
**Severidade:** Média  
**Prioridade:** Média  
**Status:** Aberto

**Descrição:**  
O `baseUrl` está hardcoded como `http://10.0.2.2:5000` (endereço do emulador Android conectando ao localhost). Em produção ou device físico, isso não funcionará.

**Passos para Reprodução:**
1. Instalar app em device físico
2. Tentar usar qualquer funcionalidade que faça chamada API

**Resultado Esperado:** App conecta ao backend correto em produção

**Resultado Obtido:** App tenta conectar a `10.0.2.2:5000` e falha

**Arquivo:** `barbearia-frontend/lib/services/api_service.dart` ou similar

---

### BUG-007 — Sem endpoint /api/auth/logout

**Módulo:** Backend (routes/auth.py)  
**Severidade:** Alta  
**Prioridade:** Alta  
**Status:** Aberto

**Descrição:**  
O endpoint de logout não existe no backend. O logout é feito 100% client-side, limpando apenas o SharedPreferences. O token JWT permanece válido no servidor por até 24h.

**Impacto:**  
- Usuário faz logout no app
- Token continua válido no servidor
- Qualquer um com o token pode continuar usando a API por até 24h

**Correção Proposta:**  
Implementar endpoint `/api/auth/logout` que:
1. Recebe o token JWT
2. Adiciona a uma blacklist
3. Retorna confirmação

---

### BUG-008 — Sem campo email em Usuario

**Módulo:** Backend (models)  
**Severidade:** Alta  
**Prioridade:** Alta  
**Status:** Aberto

**Descrição:**  
O modelo `Usuario` não possui campo `email`, impossibilitando:
- Implementação de recuperação de senha real
- Registro de novo usuário com email
- Identificação única do usuário por email

**Impacto:**  
- Fase 2 (cadastro de usuários) está bloqueada
- Recuperação de senha não pode ser implementada

**Modelo Atual:**
```
Usuario(id, username, senha_hash)
```

**Modelo Proposto:**
```
Usuario(id, username, email, senha_hash)
```

---

### BUG-009 — WhatsApp bot não implementado

**Módulo:** WhatsApp Bot  
**Severidade:** —  
**Prioridade:** Baixa  
**Status:** Aberto

**Descrição:**  
O projeto não possui implementação do bot WhatsApp. A arquitetura prevê um bot Baileys para detectar mensagens e enviar links do ChatWeb automaticamente, mas o código não foi implementado.

**Impacto:**  
- Fluxo automatizado WhatsApp → ChatWeb → App não está completo
- Usuário precisa compartilhar link manualmente

**Arquivos Previstos:** `whatsapp-bot/bot.js`

---

## Análise por Severidade

### Alta Severidade (5 bugs)
| Bug | Descrição | Impacto |
|-----|-----------|---------|
| BUG-001 | formatarTelefone() com índices errados | Telefone formatado incorretamente |
| BUG-002 | showTimes() com horários hardcodados | Horários disponíveis não respeitam configuração |
| BUG-003 | supabase-config.js vazio | ChatWeb não funciona |
| BUG-007 | Sem logout no backend | Token permanece válido após logout |
| BUG-008 | Sem email no Usuario | Bloqueia cadastro e recuperação de senha |

### Média Severidade (4 bugs)
| Bug | Descrição | Impacto |
|-----|-----------|---------|
| BUG-004 | notifications.py crash na 2ª chamada | Push para de funcionar após primeira notificação |
| BUG-005 | send_multicast() depreciado | Warning de deprecação, funciona mas não recomendado |
| BUG-006 | baseUrl hardcoded | App não funciona em device físico/produção |

---

## Recomendações

### Correção Imediata (Alta Prioridade)
1. **BUG-008** — Adicionar campo `email` ao modelo `Usuario`
2. **BUG-007** — Implementar endpoint de logout com blacklist
3. **BUG-003** — Preencher `supabase-config.js` com credenciais

### Correção Planejada
4. **BUG-001** — Corrigir índices em `formatarTelefone()`
5. **BUG-002** — Implementar cálculo dinâmico de horários em `showTimes()`
6. **BUG-004** — Adicionar verificação `firebase_admin._apps`

### Melhorias
7. **BUG-005** — Migrar para método não depreciado
8. **BUG-006** — Externalizar `baseUrl` para arquivo de configuração
9. **BUG-009** — Implementar bot WhatsApp (Baileys)

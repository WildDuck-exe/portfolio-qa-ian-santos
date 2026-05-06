# Melhorias — Klipper

## Melhorias Identificadas

### MELHORIA-001 — Implementar Cadastro de Usuários

**Módulo:** Backend / Frontend  
**Severidade:** Alta  
**Esforço:** Médio  
**Status:** Não Implementado

**Descrição:**  
O sistema não possui endpoint para registro de novos usuários. O modelo `Usuario` também não possui campo `email`, impossibilitando:
- Cadastro de barbeiros com email e senha
- Recuperação de senha via email
- Identificação única do usuário

**Impacto:**
- Sistema limitado a um único usuário admin (`admin/admin123`)
- Bloqueia implementação de Fase 2 (cadastro)

**Sugestão de Implementação:**

1. Adicionar campo `email` ao modelo `Usuario`:
```python
class Usuario(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    senha_hash = db.Column(db.String(255), nullable=False)
```

2. Criar endpoint `/api/auth/register`:
```python
@auth_bp.route('/register', methods=['POST'])
def register():
    data = request.get_json()
    # Validar email único
    # Hash da senha
    # Criar usuário
    # Retornar token JWT
```

3. Validar formato de email com regex
4. Implementar verificação de email (opcional — envios de confirmação)

---

### MELHORIA-002 — Sistema de Recuperação de Senha

**Módulo:** Backend  
**Severidade:** Alta  
**Esforço:** Médio  
**Status:** Não Implementado

**Descrição:**  
Sem campo `email` no modelo `Usuario`, não é possível implementar recuperação de senha. Atualmente o sistema só possui credenciais fixas (admin/admin123).

**Impacto:**
- Se o barbeiro esquecer a senha, não há como recuperar
- Segurança reduzida por não haver email de verificação

**Sugestão de Implementação:**

1. Adicionar campo `email` (já proposto em MELHORIA-001)
2. Criar endpoint `/api/auth/forgot-password`:
   - Receber email
   - Gerar token de recuperação (com expiração)
   - Enviar email com link de reset
3. Criar endpoint `/api/auth/reset-password`:
   - Validar token
   - Atualizar senha com novo hash

---

### MELHORIA-003 — Implementar Logout com Blacklist de Token

**Módulo:** Backend  
**Severidade:** Alta  
**Esforço:** Baixo  
**Status:** Não Implementado

**Descrição:**  
O logout atual é 100% client-side. O token JWT permanece válido no servidor por até 24h. Para um sistema de barbearia com múltiplos dispositivos (barbeiro + possivelmente assistente), isso representa um risco de segurança.

**Impacto:**
- Após logout, token ainda é válido por até 24h
- Qualquer pessoa com token pode acessar a API
- Não é possível "invalidar" sessões remotely

**Sugestão de Implementação:**

1. Criar tabela `token_blacklist`:
```python
class TokenBlacklist(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    token_jti = db.Column(db.String(36), unique=True, nullable=False)
    expirado_em = db.Column(db.DateTime, nullable=False)
```

2. Criar endpoint `/api/auth/logout`:
```python
@auth_bp.route('/logout', methods=['POST'])
def logout():
    token = get_jwt_token()  # Extrair JTI do token
    blacklist_token(token)
    return {'message': 'Logout realizado'}
```

3. Criar decorator `@token_in_blacklist` para verificar antes de processar requisições autenticadas

---

### MELHORIA-004 — Cálculo Dinâmico de Horários no Chat Web

**Módulo:** Chat Web (chat.js)  
**Severidade:** Alta  
**Esforço:** Médio  
**Status:** Não Implementado (BUG-002)

**Descrição:**  
A função `showTimes()` retorna horários hardcodados, ignorando:
- Horário de início/fim do expediente
- Pausa do meio-dia (12:00-13:00)
- Duração do serviço selecionado
- Agendamentos existentes

**Impacto:**
- Cliente pode selecionar horários inválidos
- Conflitos de agendamento
- Experiência ruim para o cliente

**Sugestão de Implementação:**

1. Criar função `calculateAvailableTimes(date, serviceId)`:
```javascript
async function calculateAvailableTimes(date, serviceId) {
    // 1. Buscar config do servidor
    const config = await fetch('/api/public/config');

    // 2. Buscar agendamentos do dia
    const bookings = await fetch(`/api/public/agendamentos?date=${date}`);

    // 3. Calcular horários disponíveis
    const available = [];
    let current = parseTime(config.horario_inicio);
    const end = parseTime(config.horario_fim);
    const pauseStart = parseTime(config.pausa_inicio);
    const pauseEnd = parseTime(config.pausa_fim);
    const duration = getServiceDuration(serviceId);

    while (current + duration <= end) {
        // Pular pausa
        if (current >= pauseStart && current < pauseEnd) {
            current = pauseEnd;
            continue;
        }
        // Pular horários ocupados
        if (!isSlotTaken(current, bookings, duration)) {
            available.push(current);
        }
        current = increment30min(current);
    }
    return available;
}
```

---

### MELHORIA-005 — Correção da Formatação de Telefone

**Módulo:** Chat Web (chat.js)  
**Severidade:** Alta  
**Esforço:** Baixo  
**Status:** Não Implementado (BUG-001)

**Descrição:**  
A função `formatarTelefone()` contém índices errados, gerando formato inválido: `(71)9-2887-024`.

**Impacto:**
- Telefone salvo incorrectamente
- Problemas em contato com cliente
- Dados inconsistentes

**Sugestão de Implementação:**
Revisar a lógica de índices da função `formatarTelefone()` e corrigir para gerar formato correto: `(71) 99999-8888`.

---

### MELHORIA-006 — Suporte a Produção com baseUrl Configurável

**Módulo:** App Flutter  
**Severidade:** Média  
**Esforço:** Baixo  
**Status:** Não Implementado (BUG-006)

**Descrição:**  
O `baseUrl` está hardcoded como `http://10.0.2.2:5000` (emulador Android). Em produção, o app precisa apontar para o servidor real.

**Impacto:**
- App não funciona em device físico
- App não funciona em produção

**Sugestão de Implementação:**

1. Usar arquivo de configuração `.env`:
```dart
final baseUrl = dotenv.env['FLUTTER_APP_BASE_URL'] ?? 'http://10.0.2.2:5000';
```

2. Ou usar conditional compilation:
```dart
final baseUrl = kIsWeb 
    ? 'https://api.klipper.app' 
    : 'http://10.0.2.2:5000';
```

3. Para releases: usar `flavor` do Flutter ou configuration build

---

### MELHORIA-007 — Implementar Bot WhatsApp (Baileys)

**Módulo:** WhatsApp Bot  
**Severidade:** —  
**Esforço:** Alto  
**Status:** Não Implementado (BUG-009)

**Descrição:**  
A arquitetura prevê um bot WhatsApp para automatizar o envio de link do ChatWeb quando um cliente manda mensagem. Currently não implementado.

**Impacto:**
- Barbeiro precisa compartilhar link manualmente
- Experiência não automatizada
- Perda de potencial clientes

**Sugestão de Implementação:**

1. Criar projeto Node.js separado (`whatsapp-bot/`)
2. Instalar dependências:
```bash
npm install @whiskeysockets/baileys qrcode-terminal axios dotenv
```

3. Implementar `bot.js` conforme documentação em `ARQUITETURA_INTEGRACAO.md`

4. Configurar como serviço no servidor para iniciar automaticamente

---

### MELHORIA-008 — Migrar para Firebase SDK Atual

**Módulo:** Backend (notifications.py)  
**Severidade:** Média  
**Esforço:** Baixo  
**Status:** Não Implementado (BUG-005)

**Descrição:**  
O código usa `send_multicast()` que está depreciado. Recomenda-se migrar para `send_each` ou `send_all`.

**Impacto:**
- Warnings de deprecação
- Potencial quebra em futuras versões do SDK

**Sugestão de Implementação:**
Substituir:
```python
# ANTES (depreciado)
response = send_multicast(message)
# DEPOIS
response = send_each(message_list)
```

---

### MELHORIA-009 — Adicionar Modelo Barbearia/Perfil

**Módulo:** Backend  
**Severidade:** Alta  
**Esforço:** Alto  
**Status:** Não Implementado

**Descrição:**  
O modelo `Usuario` não carrega dados da barbearia (nome, telefone, endereço). Fase 3 do projeto precisa de um modelo `Barbearia` separado ou extensão do `Usuario`.

**Impacto:**
- Não é possível personalizar dados da barbearia
- Branding limitado
- Fase 3 bloqueada

**Sugestão de Implementação:**

1. Criar modelo `Barbearia`:
```python
class Barbearia(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    nome = db.Column(db.String(120), nullable=False)
    telefone = db.Column(db.String(20))
    email = db.Column(db.String(120))
    endereco = db.Column(db.Text)
    usuario_id = db.Column(db.Integer, db.ForeignKey('usuario.id'))
```

2. Associar cada barbearia a um usuário admin
3. Permitir edição de dados via app

---

## Priorização Geral

| ID | Melhoria | Prioridade | Esforço |justificativa |
|----|----------|------------|---------|--------------|
| MELHORIA-001 | Cadastro de usuários | Alta | Médio | Bloqueia Fase 2 |
| MELHORIA-003 | Logout com blacklist | Alta | Baixo | Segurança crítica |
| MELHORIA-004 | Cálculo dinâmico de horários | Alta | Médio | Experiência do cliente |
| MELHORIA-005 | Correção formatação telefone | Alta | Baixo | Dados corrompidos |
| MELHORIA-002 | Recuperação de senha | Alta | Médio | Segurança |
| MELHORIA-006 | baseUrl configurável | Média | Baixo | Suporte a produção |
| MELHORIA-008 | Firebase SDK atualizado | Média | Baixo | Manutenção |
| MELHORIA-009 | Modelo Barbearia | Alta | Alto | Fase 3 bloqueada |
| MELHORIA-007 | Bot WhatsApp | Média | Alto | Automação |

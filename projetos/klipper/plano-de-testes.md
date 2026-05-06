# Plano de Testes — Klipper

## 1. Introdução

Este documento define a estratégia e o escopo dos testes para o ecossistema **Klipper**, uma plataforma de gestão de barbearia com backend Flask, app Flutter e Chat Web.

**Nota:** Este plano é baseado na análise estática da codebase (Fase 1). A execução de testes funcionais ainda não foi realizada.

---

## 2. Escopo dos Testes

### 2.1 Módulos e Funcionalidades a Testar

| Módulo | Funcionalidades | Prioridade |
|--------|-----------------|------------|
| **Autenticação** | Login, logout, restauração de sessão, registro de push token | Alta |
| **Clientes** | CRUD completo, busca por telefone | Alta |
| **Serviços** | CRUD, listagem, filtragem por categoria | Alta |
| **Agendamentos** | Criação, cancelamento, conclusão, listagem | Alta |
| **Agenda (App)** | Visualização, navegação, dashboard | Alta |
| **Chat Web** | Fluxo de agendamento, validações, formatação | Alta |
| **Notificações** | Envio de push, recepção no app | Média |
| **Bot WhatsApp** | Detecção de mensagem, resposta automática | Baixa (não implementado) |

### 2.2 Ambientes de Teste

| Ambiente | Descrição |
|----------|----------|
| **Local (Emulador)** | Flask em `localhost:5000`, Flutter em emulador Android |
| **Produção** | Backend em produção, app em device físico |

### 2.3 Escopo Não-Testado

- Testes de performance
- Testes de segurança (penetration testing)
- Testes de carga
- Integração WhatsApp (bot não implementado)

---

## 3. Estratégia de Teste

### 3.1 Tipos de Teste

| Tipo | Aplicação |
|------|----------|
| **Teste Funcional** | Validação de endpoints API e fluxos de UI |
| **Teste de Integração** | Comunicação entre módulos (Flask ↔ Supabase ↔ Flutter) |
| **Teste de UI/UX** | Verificação visual do app e Chat Web |
| **Teste de Regressão** | Validação após correções de bugs conhecidos |

### 3.2 Abordagem

- Teste manual para fluxos completos
- Teste de API via Postman/curl para endpoints
- Verificação de logs para debugging

### 3.3 Priorização

| Prioridade | Critério |
|------------|----------|
| **Alta** | Fluxos principais: login, criação de agendamento, push notification |
| **Média** | Funcionalidades auxiliares: listagens, configurações |
| **Baixa** | Features não implementadas ou cosméticas |

---

## 4. Casos de Teste Planejados

### 4.1 Autenticação

| ID | Cenário | Pré-condição | Passos | Resultado Esperado |
|----|---------|--------------|--------|-------------------|
| AUTH-001 | Login com credenciais válidas | Usuário cadastrado | 1. Inserir username e senha 2. Clicar login | Redirecionar para home, exibir token JWT |
| AUTH-002 | Login com credenciais inválidas | — | 1. Inserir username/senha incorretos 2. Clicar login | Exibir mensagem de erro |
| AUTH-003 | Login com campos vazios | — | 1. Clicar login sem preencher | Exibir validação de obrigatoriedade |
| AUTH-004 | Logout | Usuário logado | 1. Clicar logout | Limpar sessão local |
| AUTH-005 | Restauração de sessão | Token salvo | 1. Abrir app com token válido | Restaurar sessão automaticamente |
| AUTH-006 | Token expirado | Token expirado | 1. Abrir app com token inválido | Exibir tela de login |

### 4.2 Clientes

| ID | Cenário | Pré-condição | Passos | Resultado Esperado |
|----|---------|--------------|--------|-------------------|
| CLI-001 | Cadastro de cliente via app | Logado | 1. Acessar tela de clientes 2. Inserir nome e telefone 3. Salvar | Cliente salvo com sucesso |
| CLI-002 | Busca de cliente por telefone | Logado | 1. Inserir telefone na busca | Retornar dados do cliente |
| CLI-003 | Cadastro de cliente duplicado | Telefone já cadastrado | 1. Tentar cadastrar com telefone existente | Exibir erro de duplicidade |
| CLI-004 | Listagem de clientes | Logado | 1. Acessar lista de clientes | Exibir todos os clientes |

### 4.3 Serviços

| ID | Cenário | Pré-condição | Passos | Resultado Esperado |
|----|---------|--------------|--------|-------------------|
| SERV-001 | Listagem de serviços ativos | Logado | 1. Acessar tela de serviços | Listar serviços com status ativo |
| SERV-002 | Cadastro de novo serviço | Logado | 1. Inserir dados do serviço 2. Salvar | Serviço criado com sucesso |
| SERV-003 | Edição de serviço | Serviço existente | 1. Selecionar serviço 2. Alterar dados 3. Salvar | Serviço atualizado |
| SERV-004 | Desativação de serviço | Serviço existente | 1. Selecionar serviço 2. Desativar | Serviço movido para inativo |

### 4.4 Agendamentos

| ID | Cenário | Pré-condição | Passos | Resultado Esperado |
|----|---------|--------------|--------|-------------------|
| AG-001 | Criação de agendamento via ChatWeb | Cliente cadastrado, serviço existente | 1. Acessar ChatWeb 2. Preencher dados 3. Confirmar | Agendamento criado, push enviado |
| AG-002 | Criação de agendamento via App | Logado | 1. Acessar agenda 2. Selecionar cliente/serviço 3. Definir data/hora 4. Salvar | Agendamento salvo |
| AG-003 | Cancelamento de agendamento | Agendamento existente | 1. Selecionar agendamento 2. Cancelar | Status atualizado para 'cancelado' |
| AG-004 | Conclusão de agendamento | Agendamento agendado | 1. Selecionar agendamento 2. Marcar como concluído | Status atualizado para 'concluido' |
| AG-005 | Horário indisponível | Agendamento existente | 1. Tentar criar agendamento no mesmo horário | Exibir erro de conflito |

### 4.5 Chat Web

| ID | Cenário | Pré-condição | Passos | Resultado Esperado |
|----|---------|--------------|--------|-------------------|
| CW-001 | Fluxo completo de agendamento | — | 1. Acessar ChatWeb 2. Inserir nome/telefone 3. Selecionar serviço 4. Selecionar horário 5. Confirmar | Agendamento criado |
| CW-002 | Telefone inválido | — | 1. Inserir telefone mal formatado | Exibir erro de validação |
| CW-003 | Horário fora do expediente | — | 1. Tentar agendar fora do horário de funcionamento | Exibir mensagem de erro |
| CW-004 | Horário durante pausa | — | 1. Tentar agendar durante pausa (12-13h) | Bloquear horários da pausa |

### 4.6 Notificações Push

| ID | Cenário | Pré-condição | Passos | Resultado Esperado |
|----|---------|--------------|--------|-------------------|
| PUSH-001 | Recebimento de notificação | App em foreground, token registrado | 1. Criar agendamento via ChatWeb | Notificação exibida no app |
| PUSH-002 | Notificação com app em background | App minimizado | 1. Criar agendamento via ChatWeb | Notificação no tray/system |
| PUSH-003 | Push após logout | Usuário logado | 1. Fazer logout 2. Criar agendamento via ChatWeb | Verificar se notificação não é enviada (token removido) |

### 4.7 Configurações

| ID | Cenário | Pré-condição | Passos | Resultado Esperado |
|----|---------|--------------|--------|-------------------|
| CFG-001 | Alteração de horário de trabalho | Logado | 1. Acessar configurações 2. Alterar horário_inicio/fim 3. Salvar | Horários atualizados |
| CFG-002 | Cálculo de horários disponíveis | Configuração alterada | 1. Verificar horários disponíveis após alteração | Horários refletem nova configuração |

---

## 5. Bugs Known (Baseado na Análise Fase 1)

| Bug ID | Descrição | Severidade | Prioridade | Status |
|--------|-----------|-----------|------------|--------|
| BUG-001 | `formatarTelefone()` com índices errados — máscara gera `(71)9-2887-024` | Alta | Alta | Aberto |
| BUG-002 | `showTimes()` com horários hardcodados, ignora pausa/duração | Alta | Alta | Aberto |
| BUG-003 | `supabase-config.js` vazio — Chat Web não funciona via Flask | Alta | Alta | Aberto |
| BUG-004 | `notifications.py` crash na 2ª chamada (não verifica `firebase_admin._apps`) | Média | Alta | Aberto |
| BUG-005 | `send_multicast()` depreciado em notifications.py | Média | Média | Aberto |
| BUG-006 | `baseUrl` hardcoded para emulador (`http://10.0.2.2:5000`) | Média | Média | Aberto |
| BUG-007 | Sem endpoint `/api/auth/logout` — token válido após logout | Alta | Alta | Aberto |
| BUG-008 | Sem campo `email` em Usuario — impossibilita recuperação de senha | Alta | Alta | Aberto |
| BUG-009 | WhatsApp bot não existe | — | Baixa | Aberto |

---

## 6. Ambiente de Teste

### 6.1 Configuração Local

**Backend:**
```bash
cd barbearia-backend
pip install -r requirements.txt
python init_db.py
python run.py
```

**App Flutter:**
```bash
cd barbearia-frontend
flutter pub get
flutter run
```

**Credenciais:**
- Username: `admin`
- Senha: `admin123`

### 6.2 Ferramentas Recomendadas

| Ferramenta | Uso |
|------------|-----|
| Postman / Insomnia | Teste de API endpoints |
| Flutter DevTools | Debug de UI Flutter |
| Chrome DevTools | Debug de Chat Web |
| Logs Flask | Verificação de requisições |

---

## 7. Critérios de Aceite

### 7.1 Autenticação
- [ ] Login funciona com credenciais válidas
- [ ] Login falha com credenciais inválidas
- [ ] Sessão é restaurada com token válido
- [ ] Logout limpa dados locais

### 7.2 Fluxo de Agendamento
- [ ] Cliente consegue agendar via ChatWeb
- [ ] Barbeiro recebe notificação push
- [ ] Agendamento aparece no app em tempo real
- [ ] Horários indisponíveis são bloqueados corretamente

### 7.3 Chat Web
- [ ] Telefone é formatado corretamente
- [ ] Horários disponíveis respeitam configuração
- [ ] Pausa do meio-dia é bloqueada

### 7.4 Notificações
- [ ] Push funciona após primeira chamada
- [ ] Múltiplos dispositivos recebem notificação

---

## 8. Responsáveis

**Analista de QA:** Ian Santos  
**Data do Plano:** Maio 2026

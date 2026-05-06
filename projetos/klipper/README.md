# Klipper — Agenda Digital & Gestão para Barbearia

## 1. Descrição do Projeto

**Klipper** é um ecossistema completo de gestão e agendamento desenvolvido como Projeto de Extensão universitária (Engenharia de Software). O sistema resolve o problema de conciliação de agendas entre agendamentos manuais (barbeiro) e automáticos (clientes via web).

---

## 2. Escopo Funcional

### Módulos Principais

| Módulo | Descrição | Tecnologia |
|--------|-----------|------------|
| **Backend Flask** | API REST, modelos SQLAlchemy, notificações push | Python/Flask/SQLite |
| **App Flutter** | Interface do barbeiro (Android/iOS/Web) | Flutter/Dart |
| **Chat Web** | Interface do cliente para agendamento autônomo | HTML/JS |
| **Baileys Bot** | Bot WhatsApp para responder mensagens automaticamente | Node.js |

### Arquitetura do Sistema

```
CLIENTE                     SERVIDOR (Node.js)           APP DO BARBEIRO
───────                     ──────────────────           ───────────────
1. Manda "oi" no WhatsApp
   do barbeiro
         │
         ▼
   Baileys (Node.js)
   detecta mensagem
         │
         ▼ GET /api/public/config
         ├──────────────────────→ Flask retorna
         │                        whatsapp_mensagem +
         │                        chatweb_url
         │
         ▼ responde automaticamente:
   "Olá! Para agendar, acesse:
    https://chat.klipper.app"
         │
         ▼ (cliente clica no link)

2. ChatWeb abre no browser
   nome → telefone → serviço
   → data → horário → confirma
         │
         ├─ INSERT clientes ──────────────────────→ Supabase
         ├─ INSERT agendamentos ──────────────────→ Supabase
         │                                              │
         │                                              │ Realtime WS
         │                                              ▼
         │                                    App atualiza lista ✅
         │
         └─ POST /api/public/notificar ──→ Flask
                                           FCM push ──→ 📱 App
                                                   "💈 João agendou
                                                    Corte às 14:00" ✅
```

---

## 3. Stack Tecnológico

| Camada | Tecnologia |
|--------|------------|
| Backend | Python 3 / Flask / SQLAlchemy / SQLite |
| Frontend Mobile | Flutter (Android/iOS/Web) |
| Banco de Dados | SQLite (local) + Supabase (cloud/realtime) |
| Autenticação | JWT (24h) |
| Notificações | Firebase Cloud Messaging (FCM) |
| Bot WhatsApp | Baileys (Node.js) |
| Chat Web | HTML5 / JS / CSS |

---

## 4. Estrutura de Pastas

```
Projeto_Klipper/
├── barbearia-backend/         # API Flask, Modelos e Scripts de DB
│   ├── routes/                # Endpoints (Auth, Clientes, Agendamentos, Public)
│   │   ├── auth.py
│   │   ├── clientes.py
│   │   ├── agendamentos.py
│   │   └── public.py
│   ├── static/chat/           # Frontend ChatWeb
│   │   ├── index.html
│   │   └── chat.js
│   └── utils/                 # Notificações Push (Firebase)
│       └── notifications.py
├── barbearia-frontend/        # App Flutter Mobile
│   ├── lib/
│   │   ├── main.dart
│   │   ├── screens/
│   │   │   ├── login_screen.dart
│   │   │   ├── home_screen.dart
│   │   │   ├── clients_screen.dart
│   │   │   ├── agenda_screen.dart
│   │   │   ├── servicos_screen.dart
│   │   │   └── vendas_screen.dart
│   │   ├── services/
│   │   │   └── api_service.dart
│   │   └── widgets/
│   │       └── magic_bottom_nav.dart
│   └── pubspec.yaml
├── whatsapp-bot/              # Bot WhatsApp (Baileys)
│   └── bot.js
├── docs/                      # Documentação técnica
│   ├── FASE1_RELATORIO_ANALISE.md
│   └── supabase_schema.sql
├── imagens/                   # Screenshots do sistema
└── README.md
```

---

## 5. Bancos de Dados

### 5.1 SQLite (Backend Flask)

| Tabela | Modelo | Colunas Principais |
|--------|--------|-------------------|
| `usuarios` | `Usuario` | id, username, senha_hash |
| `clientes` | `Cliente` | id, nome, telefone, data_cadastro |
| `servicos` | `Servico` | id, nome, descricao, duracao_minutos, preco, categoria, ativo |
| `agendamentos` | `Agendamento` | id, cliente_id, servico_id, data_hora, observacoes, status |
| `push_tokens` | `PushToken` | id, token, dispositivo, atualizado_em |
| `configuracoes` | `Configuracao` | id, chave, valor, descricao, atualizado_em |
| `despesas` | `Despesa` | id, descricao, valor, data, categoria |

### 5.2 Supabase (Cloud/Realtime)

| Tabela | Uso |
|--------|-----|
| `clientes` | Cadastro via ChatWeb |
| `servicos` | Lista de serviços ativos |
| `agendamentos` | Agendamentos via ChatWeb (realtime habilitado) |
| `push_tokens` | Tokens FCM para notificações |

---

## 6. Endpoints da API

### 6.1 Autenticação

| Endpoint | Método | Status | Descrição |
|----------|--------|--------|----------|
| `/api/auth/login` | POST | ✅ | Login com JWT 24h |
| `/api/auth/register-token` | POST | ✅ | Registra FCM token |
| `/api/auth/logout` | qualquer | ❌ | **NÃO EXISTE** — gap crítico |

### 6.2 Endpoints Públicos (Chat Web)

| Endpoint | Método | Função |
|----------|--------|--------|
| `/api/public/validate-phone` | GET | Valida formato de telefone |
| `/api/public/cliente` | GET | Busca cliente por telefone |
| `/api/public/servicos` | GET | Lista serviços ativos |
| `/api/public/horarios` | GET | Calcula horários disponíveis |
| `/api/public/agendar` | POST | Cria agendamento + push |
| `/api/public/config` | GET | Retorna config do WhatsApp |

---

## 7. Configurações do Sistema

| Chave | Valor padrão | Uso |
|-------|-------------|-----|
| `horario_inicio` | `08:00` | Cálculo de horários |
| `horario_fim` | `18:00` | Cálculo de horários |
| `dias_trabalho` | `1,2,3,4,5,6` | Dias ativos (0=Dom) |
| `pausa_inicio` | `12:00` | Bloqueio de horário |
| `pausa_fim` | `13:00` | Bloqueio de horário |
| `whatsapp_mensagem` | Template | Mensagem automática |
| `whatsapp_mensagem_pausa` | Template | Mensagem durante pausa |
| `whatsapp_mensagem_fechado` | Template | Mensagem fora do expediente |
| `whatsapp_mensagem_cancelamento` | Template | Mensagem de cancelamento |

---

## 8. Magic Navigation (App Flutter)

Navegação inferior do app com 5 itens:

```
[0] Dashboard  — dashboard_outlined
[1] Clientes   — people_outline
[2] Agenda     — calendar_today_outlined
[3] Serviços   — content_cut_outlined
[4] Vendas     — monetization_on_outlined
```

Implementação em `lib/widgets/magic_bottom_nav.dart` — widget isolado que recebe `currentIndex` e `onTap`.

---

## 9. Problemas Conhecidos (Diagnóstico)

| # | Arquivo | Problema | Impacto |
|---|---------|----------|---------|
| 1 | `main.dart` + `config.py` | App usa Flask/SQLite, chat usa Supabase — bancos diferentes | **Crítico** |
| 2 | `chat.js` `formatarTelefone()` | Índices errados → máscara gera `(71)9-2887-024` | Alto |
| 3 | `chat.js` `showTimes()` | Horários hardcodados, ignora pausa/duração/dias | Alto |
| 4 | `static/chat/supabase-config.js` | Arquivo vazio → chat servindo pelo Flask não funciona | Alto |
| 5 | `notifications.py` | Não verifica `firebase_admin._apps` → crash na 2ª chamada | Médio |
| 6 | `notifications.py` | Usa `send_multicast()` depreciado | Médio |
| 7 | `main.dart` | `baseUrl: 'http://10.0.2.2:5000'` hardcodado para emulador | Médio |
| 8 | — | WhatsApp bot não existe no projeto | **Feature nova** |

---

## 10. Dívida Técnica

| Gap | Gravidade | Fase Impactada | Ação Necessária |
|-----|-----------|---------------|-----------------|
| Sem endpoint `/api/auth/register` | 🔴 CRÍTICO | Fase 2 | Criar no backend antes de qualquer implementação |
| Sem campo `email` em `Usuario` | 🔴 CRÍTICO | Fase 2 | Adicionar coluna email ao modelo |
| Sem endpoint `/api/auth/logout` | 🟡 MÉDIO | Fase 2 | Criar endpoint com blacklist de token |
| Sem `email` para recuperação | 🟡 MÉDIO | Fase 2 | Só estrutura placeholder é possível |
| Sem modelo `Perfil`/`Barbearia` | 🔴 CRÍTICO | Fase 3 | Criar modelo separado ou estender `Usuario` |
| Mensagens WhatsApp com branding antigo | 🟡 MÉDIO | Fase 5 | Atualizar valores em `Configuracao` |

---

## 11. Credenciais de Acesso

- **Usuário:** `admin`
- **Senha:** `admin123`

---

## 12. Autor

**Ian Santos** — Engenharia de Software  
**Instituição:** Projeto de Extensão Universitária  
**Data:** 2026

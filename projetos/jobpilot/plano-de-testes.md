# Plano de Testes — JobPilot

## 1. Introdução

Este documento define a estratégia e o escopo dos testes para o **JobPilot**, um sistema de automação de candidaturas a vagas de emprego.

**Nota:** Este plano é baseado na análise estática da codebase. A execução de testes funcionais ainda não foi realizada.

---

## 2. Escopo dos Testes

### 2.1 Módulos e Funcionalidades a Testar

| Módulo | Funcionalidades | Prioridade |
|--------|-----------------|------------|
| **Scrapers** | Descoberta de vagas, rate limiting, paginação, deduplicação | Alta |
| **Filter Pipeline** | Deduplicação, blacklist, ghost detection, scoring, repost detection | Alta |
| **AI Engine** | Geração de cover letter, seleção de currículo, análise de gaps | Alta |
| **Applicator** | Candidatura web (Playwright), candidatura email (SMTP) | Alta |
| **Database** | CRUD de vagas, atualizações, consultas por status | Alta |
| **Notifier** | Envio de notificações Telegram | Média |
| **CLI** | Comandos scrape, apply, status, report, run | Alta |
| **Dashboard** | Interface Streamlit, visualização de vagas | Média |
| **Scheduler** | Execução automática, relatórios agendados | Média |

### 2.2 Ambientes de Teste

| Ambiente | Descrição |
|----------|----------|
| **Local** | Python 3.11+, SQLite, dependências instaladas |
| **Produção** | Servidor com .env configurado |

### 2.3 Escopo Não-Testado

- Testes de performance (carga de scrapers)
- Testes de segurança (penetration testing)
- Testes de integração com plataformas externas
- Testes de UI do Dashboard Streamlit

---

## 3. Estratégia de Teste

### 3.1 Tipos de Teste

| Tipo | Aplicação |
|------|----------|
| **Teste Unitário** | Funções isoladas (scoring, filtering, formatação) |
| **Teste de Integração** | Fluxo completo scrape → filter → score → save |
| **Teste de API** | Endpoints internos (se houver) |
| **Teste de UI** | CLI commands, Dashboard Streamlit |
| **Teste de Regressão** | Validação após correções |

### 3.2 Abordagem

- Teste manual para fluxos completos
- Teste automatizado para funções de filtragem/scoring
- Verificação de logs para debugging

### 3.3 Priorização

| Prioridade | Critério |
|------------|----------|
| **Alta** | Scraping, filter pipeline, scoring |
| **Média** | Notificações, CLI, scheduler |
| **Baixa** | Dashboard, relatórios |

---

## 4. Casos de Teste Planejados

### 4.1 Scrapers

| ID | Cenário | Pré-condição | Passos | Resultado Esperado |
|----|---------|--------------|--------|-------------------|
| SCR-001 | Scrape Gupy com pagination | — | `scrape --platform gupy --max-jobs 100` | 100+ vagas coletadas |
| SCR-002 | Scrape com rate limiting | — | `scrape --platform gupy` | Sem bloqueios por rate limit |
| SCR-003 | Scrape com erro de rede | Gupy indisponível | `scrape --platform gupy` | Log de erro, sem crash |
| SCR-004 | Deduplicação de vagas | Vagas duplicadas na fonte | `scrape --platform gupy` | IDs únicos no banco |
| SCR-005 | Cache de scraping | Segunda execução mesma keyword | `scrape --platform gupy` | Vagas em cache, sem nova requisição |

### 4.2 Filter Pipeline

| ID | Cenário | Pré-condição | Passos | Resultado Esperado |
|----|---------|--------------|--------|-------------------|
| FLP-001 | Deduplicação por hash | Vaga `Nubank\|Dev` duplicada | Pipeline com dedup | 1 vaga retida |
| FLP-002 | Blacklist filter | Vaga com título contendo "estágio" | Pipeline com blacklist | Vaga removida |
| FLP-003 | Ghost detection (vaga antiga) | Vaga com data_expiracao > 30 dias | Pipeline com fantasma | Vaga marcada como fantasma |
| FLP-004 | Scoring - remote bonus | Vaga remoto | Score calculation | +20 pontos |
| FLP-005 | Scoring - empresa-alvo | Nubank | Score calculation | +15 pontos |
| FLP-006 | Scoring - repost penalty | Vaga re-publicada | Score calculation | -60 pontos |
| FLP-007 | Pipeline completo | Lista de vagas brutas | Pipeline.run() | Lista filtrada e scoreada |

### 4.3 AI Engine

| ID | Cenário | Pré-condição | Passos | Resultado Esperado |
|----|---------|--------------|--------|-------------------|
| AI-001 | Generate cover letter | Vaga válida, perfil preenchido | `ai.generate_cover_letter(vaga, perfil)` | Cover letter personalizada |
| AI-002 | Select resume | Múltiplos currículos | `ai.select_resume(vaga, perfis)` | Currículo mais relevante selecionado |
| AI-003 | Analyze gaps | Vaga com requisitos | `ai.analyze_gaps(vaga, perfil)` | Lista de gaps identificados |
| AI-004 | Provider fallback | MiniMax API indisponível | Tentar generate_cover_letter | Fallback para ResumeSelector |
| AI-005 | API key inválida | AI_API_KEY incorreta | Tentar generate_cover_letter | Log de erro, sem crash |

### 4.4 Applicator

| ID | Cenário | Pré-condição | Passos | Resultado Esperado |
|----|---------|--------------|--------|-------------------|
| APP-001 | Web apply - dry-run | Vaga Gupy | `apply --dry-run` | Log da operação, sem submissão real |
| APP-002 | Web apply - real | Credenciais corretas | `apply --platform gupy` | Candidatura enviada |
| APP-003 | Email apply | Config SMTP válida | `apply --method email` | Email enviado |
| APP-004 | SEMI_AUTO mode | Score < threshold | Modo SEMI_AUTO, score=40 | Não aplica automaticamente |
| APP-005 | SEMI_AUTO mode | Score > threshold | Modo SEMI_AUTO, score=70 | Aplica automaticamente |
| APP-006 | MANUTUAL mode | Qualquer vaga | Modo MANUAL | Requere aprovação humana |

### 4.5 Database

| ID | Cenário | Pré-condição | Passos | Resultado Esperado |
|----|---------|--------------|--------|-------------------|
| DB-001 | Salvar vaga | Vaga não existe | `save_vaga(vaga)` | Vaga persistida com ID hash |
| DB-002 | Salvar candidatura | Vaga existe | `save_candidatura(c)` | Candidatura vinculada à vaga |
| DB-003 | Atualizar status | Candidatura existente | `update_status(id, 'entrevista')` | Status atualizado |
| DB-004 | Consulta por plataforma | Várias plataformas | `get_vagas(plataforma='gupy')` | Lista filtrada |
| DB-005 | Consulta por status | Candidaturas variadas | `get_candidaturas(status='aplicada')` | Lista filtrada |
| DB-006 | Deduplicação real | Vaga duplicada via código | `save_vaga(vaga_duplicada)` | Constraint violation ou update |

### 4.6 CLI

| ID | Cenário | Pré-condição | Passos | Resultado Esperado |
|----|---------|--------------|--------|-------------------|
| CLI-001 | Comando scrape | — | `jobpilot scrape --platform gupy` | Output de vagas coletadas |
| CLI-002 | Comando status | Vagas no banco | `jobpilot status` | Dashboard de vagas/candidaturas |
| CLI-003 | Comando run --dry-run | — | `jobpilot run --dry-run` | Pipeline executado sem Apply |
| CLI-004 | Comando run com params | — | `jobpilot run --min-score 60` | Score threshold aplicado |
| CLI-005 | Help command | — | `jobpilot --help` | Lista de comandos disponíveis |

### 4.7 Notifier

| ID | Cenário | Pré-condição | Passos | Resultado Esperado |
|----|---------|--------------|--------|-------------------|
| NOT-001 | Envio de alerta | Token Telegram válido | `send_new_job_alert(vaga)` | Mensagem no Telegram |
| NOT-002 | Formato Markdown | Vaga com dados completos | `send_new_job_alert(vaga)` | Mensagem formatada corretamente |
| NOT-003 | Falha no Telegram | Token inválido | `send_new_job_alert(vaga)` | Log de erro, sem crash |

### 4.8 Scheduler

| ID | Cenário | Pré-condição | Passos | Resultado Esperado |
|----|---------|--------------|--------|-------------------|
| SCH-001 | Agendamento diário | HORARIO_AUTO_APPLY=09:00 | Deixar rodando | Pipeline executado às 09:00 |
| SCH-002 | Relatório semanal | Scheduler ativo | Esperar 1 semana | Relatório enviado |
| SCH-003 | Max daily limit | MAX_DAILY=10 | 15 vagas score acima threshold | Só 10 aplicadas |

---

## 5. Bugs Conhecidos

| Bug ID | Descrição | Severidade | Prioridade | Status |
|--------|-----------|-----------|------------|--------|
| JOBBUG-001 | AI Engine prompts não finalizados | Alta | Alta | Aberto |
| JOBBUG-002 | Scraper só Gupy implementado | Alta | Alta | Aberto |
| JOBBUG-003 | CLI limitada (só scrape/status) | Média | Média | Aberto |
| JOBBUG-004 | Sem deep analysis LLM por vaga | Alta | Alta | Aberto |
| JOBBUG-005 | Sem skill system modular | Alto | Alto | Aberto |

---

## 6. Ambiente de Teste

### 6.1 Setup

```bash
# 1. Clonar repositório
cd D:/IA/Job_automation

# 2. Criar virtual environment
python -m venv .venv
source .venv/Scripts/activate  # Linux/Mac
.venv\Scripts\activate        # Windows

# 3. Instalar dependências
pip install -r requirements.txt

# 4. Copiar .env
cp .env.example .env
# Editar .env com credenciais

# 5. Instalar Playwright browsers
playwright install
```

### 6.2 Credenciais needed

```bash
TELEGRAM_BOT_TOKEN=  # Opcional para testes
AI_API_KEY=         # Necessário para AI tests
APPLICATION_MODE=   # MANUAL para testes
```

### 6.3 Ferramentas

| Ferramenta | Uso |
|------------|-----|
| pytest | Testes unitários |
| ipdb/pdb | Debugging |
| Loguru logs | Verificação de fluxo |
| SQLite viewer | Inspeção do banco |

---

## 7. Critérios de Aceite

### 7.1 Scraping
- [ ] Gupy scraper coleta vagas com pagination
- [ ] Rate limiting não causa bloqueios
- [ ] Deduplicação funciona por hash
- [ ] Cache evita requisições duplicadas

### 7.2 Filter Pipeline
- [ ] Blacklist filtra corretamente
- [ ] Ghost detection marca vagas antigas
- [ ] Scoring aplica pesos corretos
- [ ] Repost penalty -60 funciona

### 7.3 AI Engine
- [ ] Cover letter gerada com dados da vaga
- [ ] Seleção de currículo retorna resultado
- [ ] API key inválida não causa crash

### 7.4 Applicator
- [ ] Dry-run não submete aplicação real
- [ ] SEMI_AUTO aplica acima do threshold
- [ ] MANUAL requer aprovação

### 7.5 CLI
- [ ] Todos os comandos funcionam
- [ ] Help exibe documentação
- [ ] Erros são tratados gracefully

---

## 8. Responsáveis

**Analista de QA:** Ian Santos  
**Data do Plano:** Maio 2026

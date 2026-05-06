# JobPilot — Copiloto de Candidaturas

## 1. Descrição do Projeto

**JobPilot** é um sistema de automação de candidaturas a vagas de emprego. O projeto scraping, aplicação automática, dashboard e relatórios para auxiliar profissionais na busca por emprego de forma organizada e escalável.

---

## 2. Escopo Funcional

### 2.1 Módulos Principais

| Módulo | Descrição | Tecnologia |
|--------|-----------|------------|
| **Scrapers** | Descoberta automática de vagas em múltiplas plataformas | Playwright, BeautifulSoup, lxml |
| **Filter/Pipeline** | Filtragem, deduplicação e scoring de vagas | Python (heurístico + LLM) |
| **AI Engine** | Análise de vagas, cover letter, seleção de currículo | MiniMax API (OpenAI-compatible) |
| **Applicator** | Candidatura automática (web/email) | Playwright (browser automation) |
| **Database** | Armazenamento de vagas e candidaturas | SQLite via Peewee |
| **Notifier** | Envio de notificações | Python Telegram Bot |
| **Dashboard** | Interface visual de acompanhamento | Streamlit |
| **CLI** | Interface de linha de comando | Click + Rich |

### 2.2 Funcionalidades por Módulo

#### Scrapers
- Gupy (API JSON com paginação)
- LinkedIn (Easy Apply e redirect externo)
- InHire (ATS simples)
- Indeed (agregador)
- Infojobs (agregador)
- Sólides (ATS)
- Catho (plataforma tradicional)

#### Pipeline de Filtragem
- Deduplicação por hash `empresa|titulo`
- Detecção de vagas fantasma
- Scorer com 8 critérios

#### AI Engine
- Geração de cover letter
- Seleção de currículo
- Análise de gaps
- Provider modular (troca via `.env`)

#### Modos de Aplicação
| Modo | Descrição |
|------|-----------|
| `MANUAL` | Usuário revisa e aprova cada candidatura |
| `SEMI_AUTO` | Auto-aplica em vagas com score acima do threshold |
| `AUTO` | Aplica automaticamente em todas as vagas qualificadas |

---

## 3. Stack Tecnológico

| Camada | Tecnologia |
|--------|------------|
| Scraping | Playwright, BeautifulSoup, lxml |
| Apply | Playwright (browser automation) |
| Database | SQLite via Peewee |
| IA | MiniMax API (via openai-compatible) |
| Notifier | Python Telegram Bot |
| Dashboard | Streamlit |
| CLI | Click + Rich |
| Scheduler | APScheduler |
| Logging | Loguru |

---

## 4. Estrutura de Pastas

```
Job_automation/
├── jobpilot/
│   ├── __main__.py      # Entry point (menu)
│   ├── cli.py           # Interface CLI (Click)
│   ├── dashboard.py     # Dashboard Streamlit
│   ├── config.py        # Configurações .env
│   ├── models.py        # Dataclasses
│   ├── applicator/       # Lógica de candidatura
│   │   ├── manager.py
│   │   ├── email_applicator.py
│   │   ├── web_applicator.py
│   │   └── registry.py
│   ├── database/        # SQLite via Peewee
│   ├── filter/          # Filtragem e scoring
│   │   ├── dedup.py
│   │   ├── fantasma.py
│   │   ├── scorer.py
│   │   └── pipeline.py
│   ├── notifier/        # Notificações Telegram
│   ├── reporter/        # Relatórios e scheduler
│   ├── scrapers/        # Scrapers por plataforma
│   │   ├── base.py
│   │   ├── gupy.py
│   │   └── registry.py
│   ├── monitor/         # Monitor de empresas-alvo
│   │   └── companies.py
│   └── ai_engine/       # Motor de IA
│       ├── provider.py
│       └── minimax_provider.py
├── assets/             # Recursos estáticos
├── data/                # Dados do banco
├── docs/                # Documentação
├── dist/                # Executável gerado
├── build/               # Build do PyInstaller
├── requirements.txt
├── requirements-build.txt
└── README.md
```

---

## 5. Modelos de Dados

### 5.1 Vaga
```python
class Vaga:
    id: str           # Hash SHA256 (empresa|titulo)
    empresa: str
    titulo: str
    plataforma: str
    url: str
    salario: Optional[str]
    local: Optional[str]
    tipo: Optional[str]   # remoto/híbrido/presencial
    senioridade: Optional[str]
    data_expiracao: Optional[datetime]
```

### 5.2 Candidatura
```python
class Candidatura:
    id: int
    vaga_id: str
    status: str          # aplicada/entrevista/rejeitada
    data_candidatura: datetime
    observacoes: Optional[str]
```

### 5.3 Scoring (8 critérios)
| Critério | Peso | Descrição |
|----------|------|-----------|
| Keyword match no título | +30 máx | Por palavra-chave no título |
| Modalidade | +20/-20 | Remoto (+20), híbrido (+10), presencial (-20) |
| Seniority match | +15 | Compatibilidade com nível |
| Salário informado | +5 | Présença de salário |
| Descrição completa | +10 | Completude da descrição |
| Empresa-alvo | +15 | Se é empresa monitorada |
| Densidade de keywords | +10 | Técnicas no texto |
| Repost penalty | -60 | Vaga re-publicada |

---

## 6. Variáveis de Ambiente

```bash
# API e Notificações
TELEGRAM_BOT_TOKEN=        # Token do bot Telegram
AI_API_KEY=               # Chave da API de IA

# Modo de Operação
APPLICATION_MODE=         # MANUAL, SEMI_AUTO ou AUTO

# Pipeline Automático
HORARIO_AUTO_APPLY=       # Horário de execução (ex: 09:00)
MIN_SCORE_AUTO_APPLY=     # Score mínimo (padrão: 50)
MAX_DAILY_APPLICATIONS=   # Máximo por execução (padrão: 10)
DELAY_BETWEEN_APPLICATIONS= # Segundos entre cada (padrão: 30)
SCRAPE_PLATFORMS=         # Plataformas (padrão: gupy,linkedin,indeed)
```

---

## 7. Comandos CLI

```bash
# Scrape de vagas
jobpilot scrape --platform gupy --max-jobs 50

# Aplicar a vagas (dry-run)
jobpilot apply --dry-run

# Status do banco
jobpilot status

# Enviar relatório
jobpilot report

# Pipeline completo
jobpilot run

# Pipeline com parâmetros
jobpilot run --min-score 60 --max-daily 5 --delay 45
```

---

## 8. Empresas-Alvo Monitoradas

O sistema monitora automaticamente 15 empresas:
- Nubank, PicPay, Inter, Creditas, VTEX
- e outras do setor tech/fintech

---

## 9. Gaps e Pontos de Atenção

| Gap | Severidade | Descrição |
|-----|-----------|-----------|
| AI Engine incompleto | Alto | Prompts não finalizados |
| Sem skill system | Alto | Não existe conceito modular de skills |
| Sem deep analysis LLM | Alto | Só score heurístico, sem análise por vaga |
| Sem interview prep | Alto | Skill não implementada |
| Scraper único | Alto | Só Gupy implementado |
| CLI limitado | Médio | Só scrape e status |
| Sem perfil dinâmico | Médio | USER_PROFILE hardcoded |
| Web applicator incompleto | Médio | Estrutural mas não testado |

---

## 10. Arquivo Gerado

**Executável:** `dist/jobpilot/jobpilot.exe`

Estrutura após build:
```
dist/jobpilot/
├── jobpilot.exe       ← executável
├── jobpilot/         ← código empacotado
└── (assets/ e .env devem ser movidos para esta pasta)
```

---

## 11. Autora

**Ian Santos**  
**Data:** 2026

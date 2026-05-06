# Arquitetura — JobPilot

## 1. Visão Geral da Arquitetura

JobPilot é um sistema Python de automação de candidaturas, arquitetado em camadas modulares com separação clara de responsabilidades.

```
┌─────────────────────────────────────────────────────────────┐
│                     INTERFACE (CLI/Dashboard)               │
│                   Click + Rich / Streamlit                  │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                      APPLICATION LAYER                      │
│           Scrapers │ Filter │ AI │ Applicator               │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                       DATA LAYER                           │
│                   SQLite via Peewee                        │
│              Vagas │ Candidaturas │ Empresas                │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                    EXTERNAL SERVICES                       │
│          Gupy API │ Telegram │ MiniMax IA                  │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. Fluxo de Dados

### 2.1 Pipeline Completo

```
[1] SCRAPE
    Scrapers → discovery automático de vagas
         ↓
[2] DEDUP
    Filtro de duplicatas por hash empresa|titulo
         ↓
[3] FILTER
    Blacklist → Ghost detection → Scoring
         ↓
[4] MATCH
    AI Engine: análise de compatibilidade
         ↓
[5] APPROVAL GATE
    Usuario aprova antes de aplicar
         ↓
[6] APPLY
    Web (Playwright) ou Email (SMTP)
         ↓
[7] TRACK
    Status update → Learning engine
```

### 2.2 Fluxo de Scoring

```
Vaga Recebida
     ↓
[0] Deduplicação — hash empresa|titulo
     ↓ Não é duplicata
[1] Blacklist — palavras bloqueadas
     ↓ Não bloqueado
[2] Ghost Detection — idade > threshold?
     ↓ Não é fantasma
[3] Score Heurístico (8 critérios)
     ↓
[4] Repost Detection — penalidade -60
     ↓
Score Final → Threshold check → AUTO/SEMI_AUTO/MANUAL
```

---

## 3. Módulos Principais

### 3.1 Scrapers

**Arquitetura:** Registry pattern com `BaseScraper` abstrato

```python
# jobpilot/scrapers/registry.py
SCRAPER_REGISTRY = {
    'gupy': GupyScraper,
    'linkedin': LinkedInScraper,
    'indeed': IndeedScraper,
    # ...
}
```

**Gupy Scraper (implementado):**
- API JSON (`portal.api.gupy.io/api/v1/jobs`)
- Paginação automática
- Rate limiting (2s delay)
- Error handling e logging estruturado

**LinkedIn Scraper (planejado):**
- Easy Apply detection
- Redirect handling

### 3.2 Filter Pipeline

```python
# jobpilot/filter/pipeline.py
class FilterPipeline:
    def run(self, vagas: List[Vaga]) -> List[Vaga]:
        vagas = dedup.filter(vagas)
        vagas = blacklist.filter(vagas)
        vagas = fantasma.filter(vagas)
        vagas = scorer.score(vagas)
        vagas = repost.filter(vagas)
        return vagas
```

### 3.3 AI Engine

**Interface abstrata:**
```python
# jobpilot/ai_engine/provider.py
class AIProvider(ABC):
    def generate_cover_letter(self, vaga: Vaga, perfil: dict) -> str
    def select_resume(self, vaga: Vaga, perfis: list) -> str
    def analyze_gaps(self, vaga: Vaga, perfil: dict) -> list
```

**Implementação MiniMax:**
```python
# jobpilot/ai_engine/minimax_provider.py
class MiniMaxProvider(AIProvider):
    base_url = "https://api.minimax.chat/v1"
    model = "MiniMax-Text-01"
```

### 3.4 Applicator

**Modes:**
```python
APPLICATION_MODES = {
    'MANUAL':    lambda manager, vaga: manager.requires_approval(vaga),
    'SEMI_AUTO': lambda manager, vaga: manager.auto_if_score_above(vaga, threshold),
    'AUTO':     lambda manager, vaga: manager.auto_apply(vaga),
}
```

**Registry:**
```python
# jobpilot/applicator/registry.py
APPLICATOR_REGISTRY = {
    'web': WebApplicator,
    'email': EmailApplicator,
}
```

### 3.5 Database (Peewee)

```python
# Models
class Vaga(Model):
    id = CharField(primary_key=True)  # hash SHA256
    empresa = CharField()
    titulo = CharField()
    plataforma = CharField()
    url = CharField()
    salario = CharField(null=True)
    local = CharField(null=True)
    tipo = CharField(null=True)
    senioridade = CharField(null=True)
    data_criacao = DateTimeField()
    data_expiracao = DateTimeField(null=True)

class Candidatura(Model):
    id = PrimaryKeyField()
    vaga = ForeignKeyField(Vaga)
    status = CharField()  # aplicada/entrevista/rejeitada
    data_candidatura = DateTimeField()
    observacoes = TextField(null=True)
```

---

## 4. Monitor de Empresas

```python
# jobpilot/monitor/companies.py
EMPRESAS_ALVO = [
    'Nubank', 'PicPay', 'Inter', 'Creditas', 'VTEX',
    # + 10 outras
]

class CompanyMonitor:
    def check_new_jobs(self) -> List[Vaga]:
        # Usa Playwright para monitorar carreiras
        pass
```

---

## 5. Notificações

```python
# jobpilot/notifier/telegram.py
class TelegramBot:
    def send_new_job_alert(self, vaga: Vaga) -> None:
        # Mensagem rica em Markdown
        # Ex: *"🎯 Nova vaga:*\n*Empresa:*\n*Título:*\n[Apply Now](url)"
```

---

## 6. Learning Engine

```python
# jobpilot/reporter/learning.py
class LearningEngine:
    def track(self, candidatura: Candidatura) -> None:
        # Registra resultado
        # Atualiza estatísticas
        # Identifica padrões

    def get_stats(self) -> dict:
        # Taxa de sucesso por plataforma
        # Tempo médio de resposta
        # Scores típicos das vagas aplicadas
```

---

## 7. Scheduler

```python
# jobpilot/reporter/scheduler.py
scheduler = APScheduler()
scheduler.add_job(
    'run_pipeline',
    func=pipeline.run,
    trigger='cron',
    hour=9,  # HORARIO_AUTO_APPLY
)
```

---

## 8. Configuração

```python
# jobpilot/config.py
class Config:
    TELEGRAM_BOT_TOKEN: str
    AI_API_KEY: str
    APPLICATION_MODE: str  # MANUAL/SEMI_AUTO/AUTO
    MIN_SCORE_AUTO_APPLY: int = 50
    MAX_DAILY_APPLICATIONS: int = 10
    DELAY_BETWEEN_APPLICATIONS: int = 30
    SCRAPE_PLATFORMS: list = ['gupy', 'linkedin', 'indeed']
```

---

## 9. Logging

```python
# Loguru com JSON metadata
logger.info("Vaga scrapeda", extra={
    "plataforma": "gupy",
    "empresa": "Nubank",
    "vaga_id": "abc123",
})
```

---

## 10. Gaps Arquiteturais

| Gap | Severidade | Descrição |
|-----|-----------|-----------|
| Sem skill system | Alto | Não existe arquitetura modular de skills |
| AI Engine incompleto | Alto | Prompts não finalizados |
| Sem deep analysis LLM | Alto | Só scoring heurístico |
| Sem interview prep skill | Alto | Feature não existe |
| Sem /commands | Médio | CLI limitada a scrape/status |

---

## 11. Futuras Melhorias

| Melhoria | Descrição |
|----------|-----------|
| Skill System | Arquitetura modular para adicionar skills |
| Interview Prep | Geração de perguntas e respostas prováveis |
| Deep Analysis | Análise LLM por vaga com contexto completo |
| Asset Generator | Cover letter → arquivo estruturado |
| Multi-provider | Interface para trocar provedor IA |
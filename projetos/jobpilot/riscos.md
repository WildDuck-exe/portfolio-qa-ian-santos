# Riscos — JobPilot

## 1. Riscos Técnicos

### 1.1 Scraping e Anti-Bot

| Risco | Probabilidade | Impacto | Severidade | Mitigação |
|-------|---------------|---------|------------|-----------|
| **Bloqueio por rate limiting** | Alta | Alto | 🔴 Crítico | Implementar delays configuráveis, proxies |
| **Detecção de bot pelas plataformas** | Alta | Alto | 🔴 Crítico | User-agent rotation, Human-like delays |
| **Mudança na API do Gupy** | Média | Alto | 🔴 Crítico | Monitorar breaking changes, versionar scraper |
| **Estrutura HTML modificada** | Alta | Médio | 🟡 Alto | Logging de erros, alerta de scraper quebrado |
| **Plataformas adicionam CAPTCHA** | Média | Alto | 🔴 Crítico | Playwright com detection avoidance |

### 1.2 AI Engine

| Risco | Probabilidade | Impacto | Severidade | Mitigação |
|-------|---------------|---------|------------|-----------|
| **MiniMax API indisponível** | Média | Alto | 🔴 Crítico | Fallback para ResumeSelector (já implementado) |
| **Custo de API elevado** | Média | Médio | 🟡 Alto | Limitar chamadas, cache de análises |
| **Cover letter genérica detectada** | Média | Médio | 🟡 Médio | Personalização por vaga, review humano |
| **Prompt injection em vagas** | Baixa | Alto | 🔴 Crítico | Sanitização de input antes do prompt |

### 1.3 Applicator

| Risco | Probabilidade | Impacto | Severidade | Mitigação |
|-------|---------------|---------|------------|-----------|
| **Aplicação em vaga errada** | Baixa | Alto | 🔴 Crítico | Modo MANUAL como default, review antes de aplicar |
| **Credenciais expiradas** | Média | Alto | 🔴 Crítico | Verificação periódica, alertas de expiração |
| **Formulário modificado após scrape** | Alta | Médio | 🟡 Alto | Validação de campos antes de submeter |
| **Carregamento lento causando timeout** | Média | Médio | 🟡 Médio | Timeouts configuráveis, retry logic |

### 1.4 Database

| Risco | Probabilidade | Impacto | Severidade | Mitigação |
|-------|---------------|---------|------------|-----------|
| **Banco corrompido** | Baixa | Alto | 🔴 Crítico | Backup periódico, integrity checks |
| **Colisão de hash (duplicatas)** | Muito Baixa | Alto | 🟡 Médio | Verificação adicional por URL |
| **Crescimento excessivo do banco** | Média | Baixo | 🟢 Baixo | Archiving de vagas antigas |

---

## 2. Riscos Operacionais

### 2.1 Modo AUTOMÁTICO

| Risco | Probabilidade | Impacto | Severidade | Mitigação |
|-------|---------------|---------|------------|-----------|
| **Candidaturas em excesso** | Alta | Alto | 🔴 Crítico | MAX_DAILY_APPLICATIONS configurável |
| **Aplicação em vagas irrelevantes** | Alta | Alto | 🔴 Crítico | MIN_SCORE_AUTO_APPLY alto (60+) |
| **Spam de notificações** | Alta | Médio | 🟡 Alto | Throttle de alertas, batching |
| **Ciclo de feedback negativo** | Média | Alto | 🔴 Crítico | Monitoring de taxa de rejeição |

### 2.2 Tempo e Recursos

| Risco | Probabilidade | Impacto | Severidade | Mitigação |
|-------|---------------|---------|------------|-----------|
| **Execução noturna consume recursos** | Alta | Baixo | 🟢 Baixo | Scheduling em horário de baixa activity |
| **Playwright memory leak** | Média | Médio | 🟡 Médio | Restart периодический processes |
| **Falha de rede durante scraping** | Alta | Médio | 🟡 Médio | Retry logic, resume from checkpoint |

---

## 3. Riscos de Compliance

### 3.1 Plataformas de Emprego

| Risco | Probabilidade | Impacto | Severidade | Mitigação |
|-------|---------------|---------|------------|-----------|
| **Violação de ToS das plataformas** | Alta | Alto | 🔴 Crítico | Modo MANUAL como default, disclaimer |
| **LGPD - dados de candidatos** | Média | Alto | 🔴 Crítico | Não armazenar dados sensíveis, anonimização |
| **Responsabilidade por candidatura indevida** | Média | Alto | 🔴 Crítico | Clareza no modo AUTO, consentimento explícito |

### 3.2 Automação

| Risco | Probabilidade | Impacto | Severidade | Mitigação |
|-------|---------------|---------|------------|-----------|
| **Ações não autorizadas pelo usuário** | Baixa | Alto | 🔴 Crítico | Aprovação humana explícita (SEMI_AUTO) |
| **Currículo enviado sem revisão** | Alta | Alto | 🔴 Crítico | dry-run mode, cover letter preview |

---

## 4. Riscos de Projeto

### 4.1 Funcionalidade

| Risco | Probabilidade | Impacto | Severidade | Mitigação |
|-------|---------------|---------|------------|-----------|
| **Scrapers quebrados após update** | Alta | Alto | 🔴 Crítico | Tests, alerts, versionamento |
| **AI Engine prompts ineffective** | Média | Alto | 🔴 Crítico | Avaliação contínua de quality |
| **Sem dados de vagas suficientes** | Alta | Médio | 🟡 Médio | Multi-plataforma, expansão de scrapers |
| **Score heurístico não reflete qualidade** | Média | Médio | 🟡 Médio | A/B testing, feedback loop |

### 4.2 Recursos

| Risco | Probabilidade | Impacto | Severidade | Mitigação |
|-------|---------------|---------|------------|-----------|
| **Dependência de único developer** | Alta | Alto | 🔴 Crítico | Documentação, código legível |
| **Custo de API MiniMax elevado** | Média | Alto | 🔴 Crítico | Limitar chamadas, caching |
| **Escalabilidade limitada** | Média | Médio | 🟡 Médio | Arquitetura stateless |

---

## 5. Matriz de Riscos Consolidada

| Risco | Categoria | Prob | Imp | Sev | Status |
|-------|-----------|------|-----|-----|--------|
| Bloqueio por rate limiting | Técnico | Alta | Alto | 🔴 | Mitigar |
| Detecção de bot | Técnico | Alta | Alto | 🔴 | Mitigar |
| Modo AUTO enviar demais | Operacional | Alta | Alto | 🔴 | Mitigar |
| Aplicação em vaga errada | Operacional | Baixa | Alto | 🔴 | Mitigar |
| Violação de ToS | Compliance | Alta | Alto | 🔴 | Mitigar |
| Credenciais expiradas | Técnico | Média | Alto | 🔴 | Mitigar |
| Scrapers quebrados | Funcional | Alta | Alto | 🔴 | Mitigar |
| API MiniMax indisponível | Técnico | Média | Alto | 🔴 | Mitigar |
| Spam de notificações | Operacional | Alta | Médio | 🟡 | Monitorar |
| Estrutura HTML modificada | Técnico | Alta | Médio | 🟡 | Monitorar |
| Custo de API elevado | Projeto | Média | Alto | 🔴 | Mitigar |
| Dependência de único developer | Projeto | Alta | Alto | 🔴 | Mitigar |

---

## 6. Ações de Mitigação Prioritárias

### Imediato (Esta Semana)
1. **Configurar MIN_SCORE_AUTO_APPLY=60** — evitar candidaturas irrelevantes
2. **Implementar rate limiting mais aggressivo** — delays de 5s+ entre requisições
3. **Adicionar logging de scraping** — detectar scrapers quebrados rapidamente

### Curto Prazo (Próxima Sprint)
4. **Criar modo dry-run obrigatório** — todo apply primeiro como simulation
5. **Implementar user-agent rotation** — reduzir detecção de bot
6. **Adicionar testes unitários para filter pipeline** — coverage de scoring

### Médio Prazo
7. **Implementar proxy rotation** — para scraping em escala
8. **Adicionar监控系统** — alertas de scraper quebrado
9. **Review de LGPD** — adequação de manejo de dados

---

## 7. Indicadores de Risco (KPIs)

| Indicador | Threshold | Ação |
|-----------|-----------|------|
| Taxa de bloqueios por rate limit | > 5% | Aumentar delay |
| Candidaturas por dia | > MAX_DAILY | Verificar config |
| Taxa de rejeição | > 80% | Revisar scoring |
| Scrapers quebrados | > 1 | Pausar scraping |
| Falhas de API MiniMax | > 3 consecutively | Fallback mode |

---

## 8. Autora

**Ian Santos**  
**Data:** Maio 2026

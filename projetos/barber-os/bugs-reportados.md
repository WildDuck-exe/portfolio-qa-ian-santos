# Bugs Report — BarberOS

## Resumo Executivo

| Métrica | Valor |
|---------|-------|
| **Total de Bugs** | 4 |
| **Alta Severidade** | 2 |
| **Média Severidade** | 2 |
| **Status Geral** | Todos abertos |

> Todos os bugs foram encontrados em **ambiente de produção** com dados reais de clientes.

---

## BUG-001 — Cálculo de comissão sempre fixado em 40%

| Campo | Valor |
|-------|-------|
| **ID** | BUG-001 |
| **Módulo** | Fechamento de Atendimento / Comissão |
| **Severidade** | Alta |
| **Prioridade** | Alta |
| **Status** | Aberto |
| **Reportado por** | Ian Santos |

### Descrição

Ao fechar um atendimento, o sistema aplica automaticamente **40% de comissão** para qualquer barbeiro, independente do percentual configurado no cadastro do profissional.

### Passos para Reproduzir

1. Acessar o cadastro de um barbeiro e verificar que a comissão está configurada com valor diferente de 40% (ex: 30%)
2. Criar um novo agendamento para esse barbeiro
3. Fechar o atendimento
4. Verificar o valor de comissão calculado no fechamento

### Resultado Esperado

Comissão calculada com base no **percentual configurado** no cadastro do barbeiro.

### Resultado Obtido

Comissão calculada sempre com **40%**, independente da configuração do cadastro.

### Impacto no Negócio

Barbeiros com comissão configurada abaixo de 40% recebem valores acima do acordado, gerando **prejuízo financeiro direto ao dono**. Barbeiros com comissão acima de 40% recebem menos, gerando **conflito trabalhista**. Em um cenário com 10 profissionais e média de R$ 5.000/mês em serviços, a distorção pode chegar a **R$ 2.000 a R$ 5.000/mês**.

---

## BUG-002 — Agendamentos anteriores não são exibidos

| Campo | Valor |
|-------|-------|
| **ID** | BUG-002 |
| **Módulo** | Histórico de Agendamentos |
| **Severidade** | Média |
| **Prioridade** | Alta |
| **Status** | Aberto |
| **Reportado por** | Ian Santos |

### Descrição

O sistema não exibe agendamentos de datas anteriores, impossibilitando a consulta do histórico de serviços realizados.

### Passos para Reproduzir

1. Acessar a lista de agendamentos
2. Tentar visualizar agendamentos de datas passadas (ex: semana anterior)
3. Observar que apenas agendamentos do dia atual são exibidos

### Resultado Esperado

Sistema deve exibir agendamentos de qualquer data, com filtro temporal funcional.

### Resultado Obtido

Apenas agendamentos da data atual são exibidos. Histórico inacessível.

### Impacto no Negócio

Impossibilidade de consultar histórico de serviços dificulta o controle de presença, verificação de trabalho realizado e resolução de disputas com profissionais.

---

## BUG-003 — Dificuldade para fechar agendamentos antigos

| Campo | Valor |
|-------|-------|
| **ID** | BUG-003 |
| **Módulo** | Fechamento de Agendamento |
| **Severidade** | Alta |
| **Prioridade** | Alta |
| **Status** | Aberto |
| **Reportado por** | Ian Santos |

### Descrição

O sistema apresenta dificuldade para fechar agendamentos com mais de 30 dias, bloqueando o fechamento financeiro mensal.

### Passos para Reproduzir

1. Localizar um agendamento com mais de 30 dias
2. Tentar realizar o fechamento do agendamento
3. Observar que o sistema impede ou dificulta a operação

### Resultado Esperado

Sistema deve permitir o fechamento de qualquer agendamento, independentemente da data.

### Resultado Obtido

Sistema impede o fechamento de agendamentos antigos.

### Impacto no Negócio

Bloqueio no **fechamento financeiro mensal**. Agendamentos que ficam sem fechamento não geram comissão para o profissional e não entram nos relatórios financeiros, comprometendo a conciliação do caixa.

---

## BUG-004 — Botão de exportar dados financeiros ausente no frontend

| Campo | Valor |
|-------|-------|
| **ID** | BUG-004 |
| **Módulo** | Financeiro / Interface |
| **Severidade** | Média |
| **Prioridade** | Média |
| **Status** | Aberto |
| **Reportado por** | Ian Santos |

### Descrição

O botão de exportação de dados financeiros não está visível na interface do sistema. A funcionalidade pode existir no backend, mas não é acessível pelo frontend.

### Passos para Reproduzir

1. Acessar o módulo financeiro
2. Procurar opção de exportação de dados
3. Observar que não há botão ou link disponível

### Resultado Esperado

Botão de exportação visível e funcional na interface do módulo financeiro.

### Resultado Obtido

Sem botão de exportação. Dados financeiros precisam ser extraídos manualmente.

### Impacto no Negócio

Força a equipe a realizar **manualmente a exportação de dados**, consumindo tempo e aumentando risco de erro humano na conciliação bancária.

---

## Análise por Severidade

### Alta Severidade (2 bugs)

| Bug | Descrição | Impacto |
|-----|-----------|---------|
| BUG-001 | Comissão fixada em 40% | Prejuízo financeiro direto |
| BUG-003 | Impossível fechar agendamentos antigos | Bloqueio no fechamento mensal |

### Média Severidade (2 bugs)

| Bug | Descrição | Impacto |
|-----|-----------|---------|
| BUG-002 | Histórico inacessível | Controle prejudicado |
| BUG-004 | Exportação indisponível | Processo manual |

---

## Recomendações

### Correção Imediata
1. **BUG-001** — Prioridade máxima. Impacto financeiro real e diário.
2. **BUG-003** — Desbloquear fechamento para qualquer data.

### Correção Planejada
3. **BUG-002** — Implementar filtro temporal completo.
4. **BUG-004** — Expor botão de exportação na interface.

---

**Autor:** Ian Santos — Analista QA  
**Data:** Maio 2026

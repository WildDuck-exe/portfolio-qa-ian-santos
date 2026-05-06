# BarberOS — Plano de Regressão

## 1. Introdução

Este plano de regressão tem como objetivo garantir que as correções aplicadas aos bugs identificados no BarberOS não gerem novos defeitos nas funcionalidades já existentes. Após a correção de cada bug, os cenários relacionados devem ser reexecutados.

---

## 2. Bugs Corrigidos e Pré-requisitos

| Bug ID | Descrição | Severidade | Correção Esperada |
|---|---|---|---|
| BUG-001 | Comissão calculada sempre com 40% fixo | Alta | Sistema deve aceitar percentual configurável |
| BUG-002 | Agendamentos anteriores não são exibidos | Média | Lista deve exibir agendamentos de datas passadas |
| BUG-003 | Dificuldade para fechar agendamentos antigos | Alta | Deve ser possível fechar qualquer agendamento, independentemente da data |
| BUG-004 | Botão de exportar dados ausente no frontend | Média | Botão de exportação deve estar visível e funcional |

---

## 3. Cenários de Regressão

### RG-001 — Cálculo de Comissão (Após BUG-001)

**Objetivo:** Garantir que a correção do cálculo de comissão não afete outras funcionalidades

**Pré-requisito:** Bug BUG-001 corrigido

| Step | Ação | Resultado Esperado |
|---|---|---|
| 1 | Acessar módulo de comissões | Tela de comissões carrega corretamente |
| 2 | Cadastrar serviço com percentual de comissão diferente de 40% | Sistema aceita e salva o valor configurado |
| 3 | Realizar agendamento com profissional vinculado | Comissão calculada conforme configurado |
| 4 | Verificar extrato de comissão do profissional | Valor corresponde ao percentual correto |

**Status para regressão:** Aguardando correção

---

### RG-002 — Visualização de Agendamentos Anteriores (Após BUG-002)

**Objetivo:** Garantir que agendamentos passados sejam exibidos sem regressão

**Pré-requisito:** Bug BUG-002 corrigido

| Step | Ação | Resultado Esperado |
|---|---|---|
| 1 | Acessar lista de agendamentos | Tela carrega todos os agendamentos |
| 2 | Filtrar por data de 30 dias atrás | Agendamentos antigos são exibidos corretamente |
| 3 | Visualizar detalhes de um agendamento antigo | Informações completas exibidas |
| 4 | Verificar ordenação cronológica | Lista ordenada da mais recente para a mais antiga |

**Status para regressão:** Aguardando correção

---

### RG-003 — Fechamento de Agendamentos Antigos (Após BUG-003)

**Objetivo:** Garantir que qualquer agendamento possa ser fechado sem erro

**Pré-requisito:** Bug BUG-003 corrigido

| Step | Ação | Resultado Esperado |
|---|---|---|
| 1 | Localizar agendamento com mais de 30 dias | Agendamento encontrado na lista |
| 2 | Tentar realizar fechamento do agendamento | Sistema permite fechamento sem erro |
| 3 | Verificar atualização de status | Status alterado para "Fechado" |
| 4 | Verificar reflexo em relatórios financeiros | Dados financeiros atualizados corretamente |

**Status para regressão:** Aguardando correção

---

### RG-004 — Botão de Exportação (Após BUG-004)

**Objetivo:** Garantir que a funcionalidade de exportação esteja acessível e funcional

**Pré-requisito:** Bug BUG-004 corrigido

| Step | Ação | Resultado Esperado |
|---|---|---|
| 1 | Acessar módulo financeiro | Interface carrega com botão de exportar visível |
| 2 | Clicar no botão de exportar | Sistema gera arquivo ou apresenta opções de exportação |
| 3 | Selecionar período e formato | Dados exportados corretamente |
| 4 | Verificar integridade do arquivo exportado | Arquivo abre e contém dados corretos e completos |

**Status para regressão:** Aguardando correção

---

## 4. Testes Adicionais de Regressão

Além dos cenários específicos dos bugs, os seguintes cenários de happy path devem ser reexecutados após cada correção:

| CT-ID | Descrição | Prioridade |
|---|---|---|
| TR-001 | Criar novo agendamento com dados válidos | Alta |
| TR-002 | Realizar fechamento de agendamento do dia | Alta |
| TR-003 | Verificar comissionamento em cenário normal (40%) | Alta |
| TR-004 | Exportar dados de período reciente | Média |
| TR-005 | Visualizar lista de agendamentos atuais | Alta |

---

## 5. Estratégia de Execução

1. **Antes de corrigir:** Executar todos os casos de teste uma primeira vez para estabelecer baseline
2. **Após correção de cada bug:** Executar os 4 cenários de regressão correspondentes
3. **Após todas as correções:** Executar o conjunto completo de testes de regressão
4. **Evidências:** Cada execução deve gerar evidências (prints, logs) documentadas

---

## 6. Critérios de Passagem

Para considerar o plano de regressão aprovado:
- [ ] Todos os 4 cenários de regressão (RG-001 a RG-004) passam
- [ ] Todos os 5 testes adicionais de regressão (TR-001 a TR-005) passam
- [ ] Nenhum novo bug é introduzido durante as correções
- [ ] Evidências de cada execução estão documentadas

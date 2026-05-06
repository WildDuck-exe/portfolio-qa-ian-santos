# BarberOS — Plano de Testes

## 1. Introdução

Este documento descreve o plano de testes para o **BarberOS**, um sistema de gestão para barbearias utilizado em produção por um cliente real. O sistema abrange agendamento de clientes, registro de serviços, controle de comissões e exportação de dados financeiros.

O objetivo deste plano de testes é documentar os cenários testados, os bugs identificados e os resultados obtidos durante a análise de qualidade do sistema.

---

## 2. Escopo

### O que será testado
- **Agendamento**: criação, visualização e fechamento de agendamentos
- **Comissões**: cálculo e registro de comissões sobre serviços realizados
- **Exportação de dados financeiros**: existência e funcionamento do botão de exportação
- **Histórico de agendamentos**: exibição de agendamentos antigos

### O que não será testado
- Integrações com sistemas externos de pagamento
- Funcionalidades de cadastro de clientes e profissionais (escopo não fornecido)
- Performance e carga do sistema

---

## 3. Ambiente de Teste

| Item | Descrição |
|---|---|
| Sistema | BarberOS (produção real) |
| Premissa | Sistema já em ambiente de produção com dados reais |
| Acesso | Fornecido pelo dono do sistema |

---

## 4. Bugs Conhecidos e Status

| Bug ID | Descrição | Severidade | Status |
|---|---|---|---|
| BUG-001 | Comissão calculada sempre com 40% fixo | Alta | Aberto |
| BUG-002 | Agendamentos anteriores não são exibidos | Média | Aberto |
| BUG-003 | Dificuldade para fechar agendamentos antigos | Alta | Aberto |
| BUG-004 | Botão de exportar dados ausente no frontend | Média | Aberto |

---

## 5. Casos de Teste Vinculados

| CT-ID | Descrição | Bug Vinculado | Resultado |
|---|---|---|---|
| CT-001 | Verificar cálculo de comissão para percentual diferente de 40% | BUG-001 | Falhou |
| CT-002 | Verificar visualização de agendamentos de datas anteriores | BUG-002 | Falhou |
| CT-003 | Verificar fechamento de agendamentos com mais de 30 dias | BUG-003 | Falhou |
| CT-004 | Verificar existência do botão de exportação na interface | BUG-004 | Falhou |

---

## 6. Resultados da Execução

| Status | Quantidade |
|---|---|
| Aprovado | 0 |
| Falhou | 4 |
| Bloqueado | 0 |
| Total | 4 |

**Nenhum cenário de teste passou.** Todos os casos de teste falharam, indicando Bugs funcionais que afetam directamente a operação do sistema e o negócio do cliente.

---

## 7. Critérios de Entrada e Saída

### Entrada
- Sistema BarberOS acessível em ambiente de produção
- Credenciais de acesso fornecidas pelo cliente
- Dados de agendamento e serviços existentes

### Saída
- Bugs documentados com evidências
- Relatório final com análise de impacto

---

## 8. Estratificação de Riscos

| Risco | Probabilidade | Impacto | Classificação |
|---|---|---|---|
| Commissionamento incorreto afeta pagamento dos profissionais | Alta | Alto | Crítico |
| Agendamentos antigos invisíveis geram falhas no controle | Alta | Médio | Alto |
| Dificuldade de fechamento impacta fechamento financeiro | Média | Alto | Alto |
| Falta de exportação dificulta conciliação bancária | Média | Médio | Médio |

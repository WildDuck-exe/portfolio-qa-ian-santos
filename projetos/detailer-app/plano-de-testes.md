# Plano de Testes — Detailer App

## 1. Introdução

Este documento define a estratégia e o escopo dos testes realizados na aplicação **Detailer App**, uma ferramenta mobile para gestão de negócios de detalhamento automotivo.

## 2. Escopo dos Testes

### 2.1 Escopo Funcional

| Módulo | Funcionalidades Testadas |
|--------|-------------------------|
| **Autenticação (Login/Cadastro)** | Login com credenciais válidas/inválidas, validação de campos obrigatórios, login com Google, redirecionamento para cadastro |
| **Home (Tela Inicial)** | Carregamento, exibição de cards, navegação via menu inferior |
| **Clientes** | Cadastro completo, validações de campos obrigatórios, email/telefone inválidos, preenchimento automático via CEP/ZIP |
| **Agendamentos** | Criação de agendamento, bloqueios por falta de serviço/cliente, validações |
| **Serviços** | Listagem, cadastro, ativação/desativação |

### 2.2 Escopo Não-Testado

- Integração com backend Firebase (testes realizados em ambiente de staging)
- Funcionalidades de edição de registros já existentes
- Relatórios e gráficos da tela inicial
- Testes de performance e segurança

## 3. Ambiente de Teste

| Componente | Descrição |
|------------|----------|
| Dispositivo | Emulador Android / Device físico |
| Sistema Operacional | Android |
| Versão do App | 1.0 (release testada) |
| Backend | Firebase (Firestore, Auth) |

## 4. Tipos de Teste Aplicados

- **Teste Funcional**: Validação de fluxos e regras de negócio
- **Teste de UI/UX**: Verificação de consistência visual e usabilidade
- **Teste de Validação**: Verificação de mensagens de erro e validações de campo
- **Teste de Persistência**: Verificação de gravação e recuperação de dados

## 5. Estratégia de Teste

### 5.1 Abordagem

- Teste manual baseado em roteiros (scripted testing)
- Formato BDD para cenários de teste
- Priorização por risco (impacto no negócio)

### 5.2 Critérios de Priorização

| Prioridade | Critério |
|------------|----------|
| **Alta** | Funcionalidades críticas de cadastro e autenticação |
| **Média** | Funcionalidades auxiliares (busca, navegação) |
| **Baixa** | Funcionalidades cosméticas ou secundárias |

## 6. Bugs Encontrados

| Bug ID | Descrição | Severidade | Prioridade | Status |
|--------|-----------|-----------|------------|--------|
| BUG-001 | Mensagem de erro de login em inglês | — | — | Aberto |
| BUG-002 | Inconsistência de idioma na interface | — | — | Aberto |
| BUG-003 | Mensagem incorreta para confirmação de senha vazia | — | — | Aberto |
| BUG-004 | Permite cadastro com email incompleto | — | — | Aberto |
| BUG-005 | Sem feedback após cadastro de cliente | Média | Alta | Aberto |
| BUG-006 | Cliente não aparece na lista após cadastro | Média | Alta | Aberto |
| BUG-007 | Preenchimento automático não funciona com CEP | Média | Alta | Aberto |
| BUG-008 | Permite cadastrar cliente sem telefone | Alta | Alta | Aberto |
| BUG-009 | Permite cadastrar cliente com email inválido | Média | Alta | Aberto |
| BUG-010 | Validação incorreta ao preencher apenas email | Média | Média | Aberto |
| BUG-011 | Validação incompleta com campos obrigatórios vazios | Média | Alta | Aberto |
| BUG-012 | Não valida formato do telefone | Média | Alta | Aberto |
| BUG-013 | Sem feedback após cadastro de agendamento | Média | Alta | Aberto |
| BUG-014 | Agenda não atualiza após cadastro | Alta | Alta | Aberto |
| BUG-015 | Funcionalidade de desativação de serviço indisponível | Alta | Alta | Aberto |

## 7. Resultados da Execução

| Seção | Total | PASS | FAIL | Taxa PASS |
|-------|-------|------|------|-----------|
| Login | 8 | 6 | 2 | 75% |
| Cadastro de Usuário | 11 | 9 | 2 | 82% |
| Tela Inicial | 7 | 7 | 0 | 100% |
| Cadastro de Cliente | 12 | 5 | 7 | 42% |
| Cadastro de Agendamento | 4 | 4 | 0 | 100% |
| **TOTAL** | **42** | **31** | **11** | **~74%** |

## 8. Entregáveis

- Casos de teste executados (formato BDD)
- Relatório de bugs identificados
- Recomendações de melhoria

## 9. Responsáveis

**Analista de QA:** Ian Santos  
**Data dos Testes:** Maio 2026

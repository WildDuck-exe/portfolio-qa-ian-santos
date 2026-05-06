# Detailer App — README

## 1. Descrição do Projeto

**Detailer App** é uma aplicação mobile para gestão de negócios de detailing automotivo. O sistema permite gerenciar clientes, veículos, agendamentos de serviços e despesas, sendo destinado a profissionais autônomos ou pequenas empresas do segmento de estética automotiva.

## 2. Escopo Funcional

### Módulos Principais

| Módulo | Descrição |
|--------|----------|
| **Autenticação** | Login e cadastro de usuários (profissionais de detailing) |
| **Home** | Tela inicial com resumo do dia e atalhos para principais ações |
| **Clientes** | Cadastro e gestão de clientes com dados pessoais, endereço e veículos |
| **Agendamentos** | Criação e gerenciamento de agendamentos de serviços |
| **Serviços** | Gestão dos serviços oferecidos pelo profissional |

### Funcionalidades por Módulo

#### Autenticação
- Login com email/senha
- Cadastro de nova conta
- Login com Google (em breve — não implementado)
- Validação de campos obrigatórios

#### Home
- Cards de resumo do dia
- Acesso rápido a: novo cliente, novo agendamento, nova despesa
- Exibição da agenda do dia
- Navegação pelo menu inferior

#### Clientes
- Cadastro de cliente (nome, telefone, email, endereço, CEP/ZIP code)
- Preenchimento automático de endereço via CEP/ZIP code
- Cadastro de veículo associado ao cliente
- Listagem de clientes cadastrados
- Persistência de clientes na lista

#### Agendamentos
- Novo agendamento com seleção de cliente e serviço
- Validação de campos obrigatórios
- Bloqueio quando não há serviço ou cliente com veículo cadastrado
- Feedback após cadastro

#### Serviços
- Cadastro de serviços oferecidos
- Ativação/desativação de serviços
- Listagem de serviços cadastrados

## 3. Stack Tecnológico

| Camada | Tecnologia |
|--------|----------|
| Frontend | Flutter (mobile) |
| Backend | Firebase (autenticação, Firestore) |
| Validação | Realtime Database / Cloud Functions |

## 4. Estrutura de Pastas do Projeto

```
detailer-app/
├── lib/
│   ├── main.dart
│   ├── screens/
│   │   ├── login_screen.dart
│   │   ├── register_screen.dart
│   │   ├── home_screen.dart
│   │   ├── clients_screen.dart
│   │   ├── new_client_screen.dart
│   │   ├── appointments_screen.dart
│   │   ├── new_appointment_screen.dart
│   │   └── services_screen.dart
│   ├── models/
│   │   ├── user.dart
│   │   ├── client.dart
│   │   ├── vehicle.dart
│   │   ├── appointment.dart
│   │   └── service.dart
│   ├── services/
│   │   ├── auth_service.dart
│   │   ├── database_service.dart
│   │   └── cep_service.dart
│   └── widgets/
│       └── ...
├── pubspec.yaml
└── README.md
```

## 5. Requisitos Não-Funcionais

- Aplicativo mobile (iOS/Android)
- Autenticação via Firebase
- Armazenamento de dados em Firestore
- Validação de campos em frontend
- Interface em português (com inconsistências observadas — ver bugs)

## 6. Bugs Known

| Bug ID | Descrição | Severidade | Prioridade |
|--------|-----------|-----------|------------|
| BUG-001 | Mensagem de erro de login exibida em inglês | — | — |
| BUG-002 | Inconsistência de idioma na interface | — | — |
| BUG-003 | Mensagem incorreta para campo confirmação de senha vazio | — | — |
| BUG-004 | Sistema permite cadastro com email incompleto | — | — |
| BUG-005 | Sem feedback após cadastro de cliente | Média | Alta |
| BUG-006 | Cliente não aparece na lista após cadastro sem refresh | Média | Alta |
| BUG-007 | Preenchimento automático de endereço não funciona com CEP (apenas ZIP code) | Média | Alta |
| BUG-008 | Sistema permite cadastrar cliente sem telefone | Alta | Alta |
| BUG-009 | Sistema permite cadastrar cliente com email inválido | Média | Alta |
| BUG-010 | Validação incorreta ao preencher apenas email | Média | Média |
| BUG-011 | Validação incompleta com campos obrigatórios vazios | Média | Alta |
| BUG-012 | Sistema não valida formato do telefone | Média | Alta |
| BUG-013 | Sem feedback após cadastro de agendamento | Média | Alta |
| BUG-014 | Lista/agenda não atualiza após cadastro de agendamento | Alta | Alta |
| BUG-015 | Funcionalidade de desativação de serviço indisponível | Alta | Alta |
| BUG-016 | (entrada vazia) | — | — |

## 7. Melhorias Identificadas

| ID | Módulo | Título | Impacto |
|----|--------|--------|---------|
| MELHORIA-001 | Login | Definição de idioma da aplicação | Padronizar idioma em toda a interface |
| MELHORIA-002 | Login | Implementação de login com Google | Liberar funcionalidade disponível ou ocultar botão |
| MELHORIA-003 | Cadastro | Validação de criação de conta | Implementar confirmação de email |

## 8. Resultado dos Testes

| Seção | Total | PASS | FAIL |
|-------|-------|------|------|
| Login | 8 | 6 | 2 |
| Cadastro de Usuário | 11 | 9 | 2 |
| Tela Inicial | 7 | 7 | 0 |
| Cadastro de Cliente | 12 | 5 | 7 |
| Cadastro de Agendamento | 4 | 4 | 0 |

**Taxa de sucesso geral: ~66%** (28 PASS / 42 total)

## 9. Autora do Teste

**Ian Santos** — Analista QA

## 10. Data

Maio 2026

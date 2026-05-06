# Bugs Report — Detailer App

## Resumo Executivo

| Métrica | Valor |
|---------|-------|
| **Total de Bugs** | 15 |
| **Alta Severidade** | 6 |
| **Média Severidade** | 9 |
| **Crítica** | 0 |

## Lista de Bugs

| Bug ID | Módulo | Cenário | Passos | Resultado Esperado | Resultado Obtido | Severidade | Prioridade | Status |
|--------|--------|---------|--------|-------------------|------------------|------------|------------|--------|
| **BUG-001** | Login | Login com senha inválida | 1. Inserir email válido 2. Inserir senha inválida 3. Clicar em login | O sistema deve exibir mensagem de erro em português informando credenciais inválidas | O sistema exibiu a mensagem em inglês: "Invalid login credentials". Quando apenas email preenchido, valida corretamente. Quando apenas senha preenchida, retorna erro de credenciais | — | — | Aberto |
| **BUG-002** | Login | Inconsistência de idioma na tela de login | 1. Acessar tela de login 2. Inserir email válido 3. Inserir senha inválida 4. Clicar em login | A aplicação deve manter um padrão único de idioma em toda a interface (português) | A interface apresenta mistura de idiomas: parte em português ("Cadastre-se"), parte em inglês ("Invalid login credentials", "missing email or phone") | — | — | Aberto |
| **BUG-003** | Cad. Usuario | Campo de confirmação de senha vazio | 1. Preencher nome 2. Preencher email 3. Preencher senha 4. Deixar campo "Confirmar senha" vazio 5. Clicar em cadastrar | Sistema deve informar que o campo de confirmação de senha é obrigatório | Sistema exibe mensagem "As senhas não coincidem" | — | — | Aberto |
| **BUG-004** | Cad. Usuario | Cadastro com email em formato incompleto | 1. Acessar tela de cadastro 2. Inserir nome válido 3. Inserir email inválido (pedrinhomatador@com) 4. Inserir senha válida 5. Confirmar senha 6. Clicar em "Cadastrar" | O sistema deve validar o formato do email e impedir o cadastro, exibindo mensagem "Email inválido" | O sistema permitiu o cadastro com email em formato incompleto (@com) | — | — | Aberto |
| **BUG-005** | Cad.Cliente | Falta de feedback após cadastro de cliente | 1. Acessar "Novo Cliente" 2. Preencher os campos obrigatórios com dados válidos 3. Clicar em "Salvar Cadastro Completo" | O sistema deve exibir uma mensagem de confirmação informando que o cliente foi cadastrado com sucesso | O sistema redireciona para a tela inicial sem exibir qualquer mensagem de confirmação ou feedback ao usuário | Média | Alta | Aberto |
| **BUG-006** | Cliente | Visualização de cliente recém-cadastrado na lista | 1. Cadastrar um novo cliente com dados válidos 2. Acessar a aba de clientes 3. Procurar o cliente cadastrado 4. Atualizar a página manualmente | O sistema deve exibir o cliente cadastrado na lista imediatamente após o cadastro | O cliente não aparece inicialmente na lista e só é exibido após atualização manual da página | Média | Alta | Aberto |
| **BUG-007** | Cad.Cliente | Preenchimento automático de endereço a partir do CEP | 1. Acessar tela de cadastro de cliente 2. Inserir um CEP válido (41510-535) 3. Aguardar preenchimento automático | O sistema deve preencher automaticamente os campos de endereço ao inserir um CEP válido | O sistema não realiza o preenchimento automático ao inserir um CEP válido, funcionando apenas com ZIP code | Média | Alta | Aberto |
| **BUG-008** | Cad.Cliente | Cadastro de cliente sem telefone | 1. Acessar tela "Novo Cliente" 2. Inserir apenas nome válido 3. Deixar campo telefone vazio 4. Clicar em "Salvar Cadastro Completo" | O sistema deve impedir o cadastro e informar que o telefone é obrigatório | O sistema permite cadastrar cliente sem telefone | Alta | Alta | Aberto |
| **BUG-009** | Cad.Cliente | Cadastro de cliente com email inválido | 1. Acessar tela "Novo Cliente" 2. Inserir nome válido 3. Inserir telefone válido 4. Inserir email inválido (testeDeEmail@) 5. Clicar em "Salvar Cadastro Completo" | O sistema deve validar o formato do email e impedir o cadastro, exibindo mensagem "Email inválido" | O sistema permite cadastrar cliente com email em formato inválido | Média | Alta | Aberto |
| **BUG-010** | Login | Validação incorreta ao preencher apenas o email | 1. Acessar a tela de login 2. Inserir apenas um email válido 3. Clicar em login | O sistema deve informar que a senha é obrigatória | O sistema apresentou comportamento divergente do esperado e não exibiu a validação específica de senha obrigatória | Média | Média | Aberto |
| **BUG-011** | Cad.Cliente | Validação incompleta com campos obrigatórios vazios | 1. Acessar a tela "Novo Cliente" 2. Não preencher os campos obrigatórios 3. Clicar em "Salvar Cadastro" | O sistema deve exibir mensagens de obrigatoriedade para os campos obrigatórios | O sistema não apresentou todas as validações esperadas ao tentar salvar com os campos vazios | Média | Alta | Aberto |
| **BUG-012** | Cad.Cliente | Cadastro de cliente com telefone inválido | 1. Acessar a tela "Novo Cliente" 2. Inserir nome válido 3. Inserir telefone inválido 4. Inserir email válido 5. Clicar em "Salvar Cadastro" | O sistema deve validar o formato do telefone e impedir o cadastro | O sistema não validou corretamente o formato do telefone informado | Média | Alta | Aberto |
| **BUG-013** | Cad. Agendamento | Falta de feedback após cadastro de agendamento | 1. Acessar a tela de novo agendamento 2. Preencher os dados obrigatórios válidos 3. Confirmar o cadastro | O sistema deve exibir uma mensagem de confirmação informando que o agendamento foi criado com sucesso | O sistema realizou o cadastro corretamente, porém não apresentou feedback visual. O novo agendamento só foi exibido após atualização manual da página | Média | Alta | Aberto |
| **BUG-014** | Agendamento | Lista/agenda não atualiza automaticamente após cadastro | 1. Cadastrar um novo agendamento com dados válidos 2. Acessar a agenda ou lista 3. Procurar o agendamento recém-criado 4. Atualizar a página manualmente | O sistema deve exibir o agendamento recém-cadastrado na agenda/lista imediatamente | O agendamento não aparece inicialmente e só é exibido após atualização manual da página | Alta | Alta | Aberto |
| **BUG-015** | Meus Serviços | Funcionalidade de desativação de serviço indisponível | 1. Acessar a tela "Meus Serviços" 2. Selecionar um serviço cadastrado 3. Verificar opções disponíveis 4. Tentar localizar ou executar a desativação | O sistema deve permitir a desativação de serviços cadastrados | A funcionalidade de desativação do serviço não está disponível | Alta | Alta | Aberto |

## Análise por Severidade

### Alta Severidade (6 bugs)
- **BUG-008**: Permite cadastrar cliente sem telefone — impacto direto na integridade dos dados
- **BUG-014**: Agenda não atualiza automaticamente — prejudica experiência do usuário
- **BUG-015**: Funcionalidade de desativação de serviço indisponível — bloqueia regra de negócio
- *(BUG-001, BUG-002, BUG-003, BUG-004 sem severidade classificada)*

### Média Severidade (9 bugs)
- Bugs de validação de campo (BUG-005, BUG-009, BUG-010, BUG-011, BUG-012, BUG-013)
- Bugs de usabilidade (BUG-006, BUG-007)

## Análise por Prioridade

### Alta Prioridade (13 bugs)
- BUG-005, BUG-006, BUG-007, BUG-008, BUG-009, BUG-011, BUG-012, BUG-013, BUG-014, BUG-015
- *(BUG-001, BUG-002, BUG-003, BUG-004 sem prioridade classificada)*

### Média Prioridade (1 bug)
- BUG-010

## Recomendações

1. **Correção imediata**: BUG-008, BUG-014, BUG-015 (impactam regras de negócio)
2. **Correção urgente**: BUG-005, BUG-013 (feedback ao usuário)
3. **Correção planejada**: Bugs de validação (BUG-009, BUG-010, BUG-011, BUG-012)
4. **Padronização de idioma**: Unificar mensagens em português (BUG-001, BUG-002)

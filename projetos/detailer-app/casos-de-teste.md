# Casos de Teste — Detailer App

## Login

### CT-001 — Login com Sucesso

**Módulo:** Login  
**Título:** Login com Sucesso

**Cenário (BDD):**  
Dado que o usuário possui credenciais válidas e está na tela de login  
Quando ele informa email e senha válidos e clica em login  
Então o sistema deve autenticar o usuário e redirecioná-lo para a tela inicial

**Pré-condição:** Usuário cadastrado

**Passos:**
1. Inserir email válido
2. Inserir senha válida
3. Clicar login

**Resultado esperado:** O sistema deve autenticar o usuário e redirecionar para a tela inicial, exibindo feedback visual de sucesso.

**Resultado obtido:** Sistema autenticou o usuário e redirecionou para a tela inicial conforme esperado.

**Status:** PASS

---

### CT-002 — Login com senha inválida

**Módulo:** Login  
**Título:** Login com senha inválida

**Cenário (BDD):**  
Dado que o usuário está na tela de login  
Quando ele informa um email válido, uma senha inválida e clica em login  
Então o sistema deve impedir o acesso e exibir uma mensagem de credenciais inválidas

**Pré-condição:** Usuário cadastrado

**Passos:**
1. Inserir email válido
2. Inserir senha inválida
3. Clicar login

**Resultado esperado:** Sistema deve exibir mensagem de erro informando credenciais inválidas

**Resultado obtido:** Sistema bloqueou o acesso e exibiu mensagem de erro ao informar senha inválida.

**Status:** PASS

---

### CT-003 — Login com email inválido

**Módulo:** Login  
**Título:** Login com email inválido

**Cenário (BDD):**  
Dado que o usuário está na tela de login  
Quando ele informa um email inválido, uma senha válida e clica em login  
Então o sistema deve impedir o acesso e exibir uma mensagem de credenciais inválidas

**Pré-condição:** Usuário cadastrado

**Passos:**
1. Inserir email inválido
2. Inserir senha válida
3. Clicar login

**Resultado esperado:** Sistema deve exibir mensagem de erro informando credenciais inválidas

**Resultado obtido:** Sistema bloqueou o acesso e exibiu mensagem de erro ao informar email inválido.

**Status:** PASS

---

### CT-004 — Login sem preenchimento dos campos

**Módulo:** Login  
**Título:** Login sem preenchimento dos campos

**Cenário (BDD):**  
Dado que o usuário está na tela de login  
Quando ele clica em login sem preencher os campos obrigatórios  
Então o sistema deve exibir a validação de obrigatoriedade dos campos

**Pré-condição:** Usuário cadastrado

**Passos:**
1. Clicar login

**Resultado esperado:** Sistema deve exibir mensagem de validação dos campos obrigatórios

**Resultado obtido:** Sistema exibiu as validações de obrigatoriedade ao tentar logar sem preencher os campos.

**Status:** PASS

---

### CT-005 — Apenas email preenchido

**Módulo:** Login  
**Título:** Apenas email preenchido

**Cenário (BDD):**  
Dado que o usuário está na tela de login  
Quando ele informa apenas um email válido e clica em login  
Então o sistema deve informar que a senha é obrigatória

**Pré-condição:** Usuário cadastrado

**Passos:**
1. Inserir email válido
2. Clicar login

**Resultado esperado:** Sistema deve exibir mensagem informando que a senha é obrigatória

**Resultado obtido:** Sistema apresentou comportamento diferente do esperado ao preencher apenas o email; a validação de senha obrigatória não ficou coerente.

**Status:** FAIL — BUG-010

---

### CT-006 — Apenas senha preenchida

**Módulo:** Login  
**Título:** Apenas senha preenchida

**Cenário (BDD):**  
Dado que o usuário está na tela de login  
Quando ele informa apenas uma senha válida e clica em login  
Então o sistema deve informar que o email é obrigatório

**Pré-condição:** Usuário cadastrado

**Passos:**
1. Inserir senha válida
2. Clicar login

**Resultado esperado:** Sistema deve exibir mensagem informando que o email é obrigatório

**Resultado obtido:** Sistema exibiu a validação esperada ao tentar logar preenchendo apenas a senha.

**Status:** PASS

---

### CT-007 — Login com Google

**Módulo:** Login  
**Título:** Login com Google

**Cenário (BDD):**  
Dado que o usuário está na tela de login  
Quando ele clica no botão "Continue with Google"  
Então o sistema deve iniciar o fluxo de autenticação com o Google

**Pré-condição:** Usuário na tela de login

**Passos:**
1. Clicar no botão "Continue with Google"

**Resultado esperado:** O sistema deve iniciar o fluxo de autenticação com o Google, redirecionando o usuário para seleção de conta ou exibindo interface de login do Google.

**Resultado obtido:** Botão acionado sem erro aparente durante a execução registrada.

**Status:** PASS

---

### CT-008 — Redirecionamento para cadastro

**Módulo:** Login  
**Título:** Redirecionamento para cadastro

**Cenário (BDD):**  
Dado que o usuário está na tela de login  
Quando ele clica no botão "Cadastre-se"  
Então o sistema deve redirecioná-lo para a tela de cadastro

**Pré-condição:** Usuário na tela de login

**Passos:**
1. Clicar no botão "Cadastre-se"

**Resultado esperado:** O sistema deve redirecionar para a tela de cadastro

**Resultado obtido:** Sistema redirecionou corretamente para a tela de cadastro.

**Status:** PASS

---

## Cadastro de Usuário

### CT-009 — Navegação de volta

**Módulo:** Cad. Usuario  
**Título:** Navegação de volta (se existir)

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro  
Quando ele clica na seta de voltar  
Então o sistema deve redirecioná-lo para a tela de login

**Pré-condição:** Usuário na tela de cadastro

**Passos:**
1. Clicar na seta no canto superior esquerdo

**Resultado esperado:** O sistema deve redirecionar para a tela de login

**Resultado obtido:** Sistema retornou corretamente para a tela de login.

**Status:** PASS

---

### CT-010 — Cadastro com Sucesso

**Módulo:** Cad. Usuario  
**Título:** Cadastro com Sucesso

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro  
Quando ele preenche nome, email, senha e confirmação válidos e clica em "Cadastrar"  
Então o sistema deve criar a conta, autenticar o usuário e redirecioná-lo para a tela inicial

**Pré-condição:** Usuário na tela de cadastro

**Passos:**
1. Inserir nome completo
2. Inserir email válido
3. Inserir senha válida
4. Confirmar senha válida
5. Clicar "Cadastrar"

**Resultado esperado:** O sistema deve cadastrar, autenticar o usuário e redirecionar para a tela inicial, junto com uma mensagem de "Conta criada com Sucesso!"

**Resultado obtido:** Conta criada com sucesso, usuário autenticado e redirecionado para a tela inicial.

**Status:** PASS

---

### CT-011 — Cadastro com todos os campos vazios

**Módulo:** Cad. Usuario  
**Título:** Cadastro com todos os campos vazios

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro  
Quando ele clica em "Cadastrar" sem preencher nenhum campo  
Então o sistema deve exibir as validações de obrigatoriedade para todos os campos obrigatórios

**Pré-condição:** Usuário na tela de cadastro

**Passos:**
1. Clicar "Cadastrar"

**Resultado esperado:** O sistema deve marcar e mostrar uma mensagem em todos os campos obrigatórios

**Resultado obtido:** Sistema exibiu validações nos campos obrigatórios ao tentar cadastrar sem preenchimento.

**Status:** PASS

---

### CT-012 — Cadastro sem nome

**Módulo:** Cad. Usuario  
**Título:** Cadastro sem nome

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro  
Quando ele preenche email, senha e confirmação, mas deixa o nome vazio e clica em "Cadastrar"  
Então o sistema deve informar que o nome é obrigatório

**Pré-condição:** Usuário na tela de cadastro

**Passos:**
1. Inserir email válido
2. Inserir senha válida
3. Confirmar senha válida
4. Clicar "Cadastrar"

**Resultado esperado:** O sistema deve informar "Nome é obrigatório"

**Resultado obtido:** Sistema exibiu a mensagem de obrigatoriedade do nome.

**Status:** PASS

---

### CT-013 — Cadastro sem email

**Módulo:** Cad. Usuario  
**Título:** Cadastro sem email

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro  
Quando ele preenche nome, senha e confirmação, mas deixa o email vazio e clica em "Cadastrar"  
Então o sistema deve informar que o email é obrigatório

**Pré-condição:** Usuário na tela de cadastro

**Passos:**
1. Inserir nome válido
2. Inserir senha válida
3. Confirmar senha válida
4. Clicar "Cadastrar"

**Resultado esperado:** O sistema deve informar "Email é obrigatório"

**Resultado obtido:** Sistema exibiu a mensagem de obrigatoriedade do email.

**Status:** PASS

---

### CT-014 — Cadastro sem senha

**Módulo:** Cad. Usuario  
**Título:** Cadastro sem senha

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro  
Quando ele preenche nome e email, mas deixa a senha vazia e clica em "Cadastrar"  
Então o sistema deve informar que a senha é obrigatória

**Pré-condição:** Usuário na tela de cadastro

**Passos:**
1. Inserir nome válido
2. Inserir email
3. Clicar "Cadastrar"

**Resultado esperado:** O sistema deve informar "Senha é obrigatória"

**Resultado obtido:** Sistema exibiu a mensagem de obrigatoriedade da senha.

**Status:** PASS

---

### CT-015 — Cadastro sem confirmação de senha

**Módulo:** Cad. Usuario  
**Título:** Cadastro sem confirmação de senha

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro  
Quando ele preenche nome, email e senha, deixa a confirmação vazia e clica em "Cadastrar"  
Então o sistema deve informar que a confirmação de senha é obrigatória

**Pré-condição:** Usuário na tela de cadastro

**Passos:**
1. Inserir nome completo
2. Inserir email válido
3. Inserir senha válida
4. Clicar "Cadastrar"

**Resultado esperado:** O sistema deve informar "Confirme sua senha"

**Resultado obtido:** Ao deixar a confirmação de senha vazia, o sistema exibiu a mensagem "As senhas não coincidem", em vez de informar que o campo é obrigatório.

**Status:** FAIL — BUG-003

---

### CT-016 — Cadastro com email inválido

**Módulo:** Cad. Usuario  
**Título:** Cadastro com email inválido

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro  
Quando ele informa um email inválido e tenta concluir o cadastro  
Então o sistema deve bloquear o cadastro e exibir uma mensagem de email inválido

**Pré-condição:** Usuário na tela de cadastro

**Passos:**
1. Inserir nome completo
2. Inserir email inválido
3. Inserir senha válida
4. Confirmar senha válida
5. Clicar "Cadastrar"

**Resultado esperado:** O sistema deve exibir mensagem "Email Inválido"

**Resultado obtido:** O sistema permitiu o cadastro com email em formato incompleto (@com).

**Status:** FAIL — BUG-004

---

### CT-017 — Cadastro com email já existente

**Módulo:** Cad. Usuario  
**Título:** Cadastro com email já existente

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro  
Quando ele informa um email já cadastrado e tenta criar uma nova conta  
Então o sistema deve impedir o cadastro e informar que o email já existe

**Pré-condição:** Usuário na tela de cadastro

**Passos:**
1. Inserir nome completo
2. Inserir email já existente
3. Inserir senha válida
4. Confirmar senha válida
5. Clicar "Cadastrar"

**Resultado esperado:** O sistema deve informar "Email já cadastrado"

**Resultado obtido:** Sistema exibiu a mensagem informando que o email já está cadastrado.

**Status:** PASS

---

### CT-018 — Senha e confirmação diferentes

**Módulo:** Cad. Usuario  
**Título:** Senha e confirmação diferentes

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro  
Quando ele informa senha e confirmação diferentes e clica em "Cadastrar"  
Então o sistema deve impedir o cadastro e informar que as senhas não coincidem

**Pré-condição:** Usuário na tela de cadastro

**Passos:**
1. Inserir nome completo
2. Inserir email válido
3. Inserir senha válida
4. Inserir senha diferente na confirmação
5. Clicar "Cadastrar"

**Resultado esperado:** O sistema deve informar que os campos de senha não coincidem.

**Resultado obtido:** Sistema impediu o cadastro e informou que as senhas não coincidem.

**Status:** PASS

---

### CT-019 — Senha muito curta

**Módulo:** Cad. Usuario  
**Título:** Senha muito curta

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro  
Quando ele informa uma senha fora da política mínima exigida e clica em "Cadastrar"  
Então o sistema deve impedir o cadastro e informar a regra de senha

**Pré-condição:** Usuário na tela de cadastro

**Passos:**
1. Inserir nome completo
2. Inserir email válido
3. Inserir senha inválida (curta)
4. Confirmar senha inválida
5. Clicar "Cadastrar"

**Resultado esperado:** O sistema deve informar "A senha deve ter no mínimo 6 digitos, 1 Maiúscula e 1 Número"

**Resultado obtido:** Sistema bloqueou o cadastro e exibiu a regra de senha mínima esperada.

**Status:** PASS

---

## Tela Inicial

### CT-020 — Carregamento da tela inicial

**Módulo:** Home  
**Título:** Carregamento da tela inicial

**Cenário (BDD):**  
Dado que o usuário está autenticado no sistema  
Quando a tela inicial é carregada após o login  
Então o sistema deve exibir corretamente os cards, atalhos e o menu inferior

**Pré-condição:** Usuário autenticado no sistema

**Passos:**
1. Realizar login com credenciais válidas
2. Aguardar carregamento da tela inicial

**Resultado esperado:** O sistema deve carregar corretamente a tela inicial, exibindo o nome do usuário, os cards de resumo do dia, os atalhos principais e o menu inferior, sem travamentos ou falhas visuais.

**Resultado obtido:** Tela inicial carregada corretamente, com cards, atalhos e menu inferior visíveis.

**Status:** PASS

---

### CT-021 — Exibição dos cards de resumo do dia

**Módulo:** Home  
**Título:** Exibição dos cards de resumo do dia

**Cenário (BDD):**  
Dado que o usuário está na tela inicial  
Quando ele visualiza os cards de resumo do dia  
Então o sistema deve exibir os valores sem campos vazios, quebras indevidas ou inconsistências

**Pré-condição:** Usuário autenticado na tela inicial

**Passos:**
1. Acessar a tela inicial
2. Observar os cards "Pendentes Hoje" e "Receita Hoje"

**Resultado esperado:** O sistema deve exibir os cards de resumo com valores carregados corretamente, sem campos vazios, textos quebrados ou informações inconsistentes.

**Resultado obtido:** Cards "Pendentes Hoje" e "Receita Hoje" foram exibidos corretamente na tela inicial.

**Status:** PASS

---

### CT-022 — Acesso ao cadastro de novo cliente

**Módulo:** Home  
**Título:** Acesso ao cadastro de novo cliente

**Cenário (BDD):**  
Dado que o usuário está na tela inicial  
Quando ele clica no botão "Novo Cliente"  
Então o sistema deve redirecioná-lo para a tela de cadastro de cliente

**Pré-condição:** Usuário autenticado na tela inicial

**Passos:**
1. Acessar a tela inicial
2. Clicar no botão "Novo Cliente"

**Resultado esperado:** O sistema deve redirecionar o usuário para a tela de cadastro de cliente, carregando os campos e ações esperadas sem erro.

**Resultado obtido:** Sistema abriu corretamente a tela de cadastro de cliente.

**Status:** PASS

---

### CT-023 — Acesso ao cadastro de novo agendamento

**Módulo:** Home  
**Título:** Acesso ao cadastro de novo agendamento

**Cenário (BDD):**  
Dado que o usuário está na tela inicial  
Quando ele clica no botão "Novo Agend."  
Então o sistema deve redirecioná-lo para a tela de cadastro de agendamento

**Pré-condição:** Usuário autenticado na tela inicial

**Passos:**
1. Acessar a tela inicial
2. Clicar no botão "Novo Agend."

**Resultado esperado:** O sistema deve redirecionar o usuário para a tela de agendamento, carregando os campos e ações esperadas sem erro.

**Resultado obtido:** Sistema abriu corretamente a tela de cadastro de agendamento.

**Status:** PASS

---

### CT-024 — Acesso ao cadastro de nova despesa

**Módulo:** Home  
**Título:** Acesso ao cadastro de nova despesa

**Cenário (BDD):**  
Dado que o usuário está na tela inicial  
Quando ele clica no botão "Nova Despesa"  
Então o sistema deve redirecioná-lo para a tela de cadastro de despesa

**Pré-condição:** Usuário autenticado na tela inicial

**Passos:**
1. Acessar a tela inicial
2. Clicar no botão "Nova Despesa"

**Resultado esperado:** O sistema deve redirecionar o usuário para a tela de cadastro de despesa, carregando os campos e ações esperadas sem erro.

**Resultado obtido:** Sistema abriu corretamente a tela de cadastro de despesa.

**Status:** PASS

---

### CT-025 — Exibição da agenda do dia

**Módulo:** Home  
**Título:** Exibição da agenda do dia

**Cenário (BDD):**  
Dado que o usuário está na tela inicial  
Quando ele visualiza a seção "Agenda de Hoje"  
Então o sistema deve exibir uma mensagem coerente com o estado da agenda do dia

**Pré-condição:** Usuário autenticado na tela inicial

**Passos:**
1. Acessar a tela inicial
2. Observar a seção "Agenda de Hoje (Pendentes)"

**Resultado esperado:** O sistema deve exibir a mensagem de agenda livre ou tudo concluído de forma coerente com a ausência de pendências no dia.

**Resultado obtido:** Sistema exibiu mensagem coerente com o estado atual da agenda do dia.

**Status:** PASS

---

### CT-026 — Navegação pelo menu inferior

**Módulo:** Home  
**Título:** Navegação pelo menu inferior da tela inicial

**Cenário (BDD):**  
Dado que o usuário está na tela inicial  
Quando ele navega por cada opção do menu inferior  
Então o sistema deve redirecioná-lo corretamente para cada módulo

**Pré-condição:** Usuário autenticado na tela inicial

**Passos:**
1. Acessar a tela inicial
2. Clicar em cada opção do menu inferior, uma por vez (Home; Clientes; Agenda e Produção; Financeiro)

**Resultado esperado:** O sistema deve redirecionar o usuário para cada opção de tela, carregando os campos e ações esperadas sem erro.

**Resultado obtido:** Todas as opções do menu inferior redirecionaram corretamente para seus respectivos módulos.

**Status:** PASS

---

## Cadastro de Cliente

### CT-027 — Cadastro de cliente com sucesso (Sem endereço, sem veículo)

**Módulo:** Cad. Cliente  
**Título:** Cadastro de cliente com sucesso (Sem endereço, sem veículo)

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro de cliente  
Quando ele preenche os dados obrigatórios sem endereço e sem veículo e clica em "Salvar Cadastro"  
Então o sistema deve salvar o cliente com sucesso e exibir feedback visual de confirmação

**Pré-condição:** Usuário autenticado na tela de cadastro de cliente

**Passos:**
1. Inserir nome completo
2. Inserir telefone
3. Inserir email válido
4. Selecionar uma das opções de "Como nos conheceu?"
5. Clicar em "Salvar Cadastro"

**Resultado esperado:** O sistema deve salvar o cadastro do cliente com sucesso e exibir feedback visual de confirmação.

**Resultado obtido:** Cliente foi cadastrado, porém o sistema redirecionou para a tela inicial sem exibir mensagem de confirmação ao usuário.

**Status:** FAIL — BUG-005

---

### CT-028 — Cadastro de cliente com sucesso (Com endereço, sem veículo)

**Módulo:** Cad. Cliente  
**Título:** Cadastro de cliente com sucesso (Com endereço, sem veículo)

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro de cliente  
Quando ele preenche os dados obrigatórios, informa um CEP ou ZIP code válido e clica em "Salvar Cadastro"  
Então o sistema deve salvar o cliente com sucesso e exibir feedback visual de confirmação

**Pré-condição:** Usuário autenticado na tela de cadastro de cliente

**Passos:**
1. Inserir nome completo
2. Inserir telefone
3. Inserir email válido
4. Selecionar uma das opções de "Como nos conheceu?"
5. Inserir CEP ou ZIPcode
6. Conferir se os campos foram preenchidos corretamente
7. Clicar em "Salvar Cadastro"

**Resultado esperado:** O sistema deve salvar o cadastro do cliente com sucesso e exibir feedback visual de confirmação.

**Resultado obtido:** Cliente foi cadastrado, porém o sistema redirecionou para a tela inicial sem exibir mensagem de confirmação ao usuário.

**Status:** FAIL — BUG-005

---

### CT-029 — Verificar persistência de cliente cadastrado

**Módulo:** Cad. Cliente  
**Título:** Verificar persistência de cliente cadastrado

**Cenário (BDD):**  
Dado que existe um cliente cadastrado com sucesso  
Quando o usuário acessa a aba "Clientes"  
Então o sistema deve exibir o cliente recém-cadastrado na lista

**Pré-condição:** Cliente cadastrado com sucesso

**Passos:**
1. Clicar na aba de "Clientes" no menu inferior
2. Visualizar se o cliente está cadastrado

**Resultado esperado:** O cliente cadastrado deve ser exibido na tela de clientes cadastrados

**Resultado obtido:** Cliente cadastrado foi exibido na listagem de clientes.

**Status:** PASS

---

### CT-030 — Cadastro de cliente com todos os campos vazios

**Módulo:** Cad. Cliente  
**Título:** Cadastro de cliente com todos os campos vazios

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro de cliente  
Quando ele clica em "Salvar Cadastro" sem preencher os campos obrigatórios  
Então o sistema deve exibir as mensagens de validação para os campos obrigatórios

**Pré-condição:** Usuário autenticado na tela de cadastro de cliente

**Passos:**
1. Clicar em "Salvar Cadastro"

**Resultado esperado:** Sistema deve exibir mensagem coerente com os campos da tela, como: "Nome é obrigatório" "Telefone obrigatório"

**Resultado obtido:** Ao tentar salvar com os campos vazios, o sistema não apresentou todas as validações esperadas para os campos obrigatórios.

**Status:** FAIL — BUG-011

---

### CT-031 — Cadastro de cliente sem nome

**Módulo:** Cad. Cliente  
**Título:** Cadastro de cliente sem nome

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro de cliente  
Quando ele preenche telefone e email, mas deixa o nome vazio e clica em "Salvar Cadastro"  
Então o sistema deve informar que o nome é obrigatório

**Pré-condição:** Usuário autenticado na tela de cadastro de cliente

**Passos:**
1. Inserir telefone
2. Inserir email válido
3. Selecionar uma das opções de "Como nos conheceu?"
4. Clicar em "Salvar Cadastro"

**Resultado esperado:** Sistema deve exibir mensagem: "Nome obrigatório"

**Resultado obtido:** Sistema exibiu a mensagem de obrigatoriedade do nome ao tentar salvar o cadastro.

**Status:** PASS

---

### CT-032 — Cadastro de cliente sem telefone

**Módulo:** Cad. Cliente  
**Título:** Cadastro de cliente sem telefone

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro de cliente  
Quando ele preenche nome e email, mas deixa o telefone vazio e clica em "Salvar Cadastro"  
Então o sistema deve informar que o telefone é obrigatório

**Pré-condição:** Usuário autenticado na tela de cadastro de cliente

**Passos:**
1. Inserir nome completo
2. Inserir email válido
3. Selecionar uma das opções de "Como nos conheceu?"
4. Clicar em "Salvar Cadastro"

**Resultado esperado:** Sistema deve exibir mensagem: "Telefone obrigatório"

**Resultado obtido:** O sistema permitiu cadastrar cliente sem preencher o telefone.

**Status:** FAIL — BUG-008

---

### CT-033 — Cadastro de cliente sem email

**Módulo:** Cad. Cliente  
**Título:** Cadastro de cliente sem email

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro de cliente  
Quando ele preenche nome e telefone, deixa o email vazio e clica em "Salvar Cadastro"  
Então o sistema deve permitir o cadastro do cliente

**Pré-condição:** Usuário autenticado na tela de cadastro de cliente

**Passos:**
1. Inserir nome completo
2. Inserir telefone
3. Selecionar uma das opções de "Como nos conheceu?"
4. Clicar em "Salvar Cadastro"

**Resultado esperado:** O sistema deve conseguir efetivar o cadastro.

**Resultado obtido:** Sistema permitiu o cadastro do cliente mesmo sem o preenchimento do email.

**Status:** PASS

---

### CT-034 — Cadastro de cliente com email inválido

**Módulo:** Cad. Cliente  
**Título:** Cadastro de cliente com email inválido

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro de cliente  
Quando ele preenche nome e telefone, insere um email inválido e clica em "Salvar Cadastro"  
Então o sistema deve validar o formato do email e impedir o cadastro

**Pré-condição:** Usuário autenticado na tela de cadastro de cliente

**Passos:**
1. Inserir nome completo
2. Inserir telefone
3. Inserir email inválido
4. Selecionar uma das opções de "Como nos conheceu?"
5. Clicar em "Salvar Cadastro"

**Resultado esperado:** Sistema deve exibir mensagem: "Email inválido"

**Resultado obtido:** O sistema permitiu cadastrar cliente com email em formato inválido.

**Status:** FAIL — BUG-009

---

### CT-035 — Cadastro de cliente com telefone inválido

**Módulo:** Cad. Cliente  
**Título:** Cadastro de cliente com telefone inválido

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro de cliente  
Quando ele preenche nome, email e um telefone inválido e clica em "Salvar Cadastro"  
Então o sistema deve validar o formato do telefone e impedir o cadastro

**Pré-condição:** Usuário autenticado na tela de cadastro de cliente

**Passos:**
1. Inserir nome completo
2. Inserir telefone inválido
3. Inserir email válido
4. Selecionar uma das opções de "Como nos conheceu?"
5. Clicar em "Salvar Cadastro"

**Resultado esperado:** Sistema deve validar o formato do telefone e impedir o cadastro.

**Resultado obtido:** O sistema não validou corretamente o formato do telefone informado.

**Status:** FAIL — BUG-012

---

### CT-036 — Preenchimento automático de endereço com ZIP code válido

**Módulo:** Cad. Cliente  
**Título:** Preenchimento automático de endereço com ZIP code válido

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro de cliente  
Quando ele insere um ZIP code válido (formato US)  
Então o sistema deve preencher automaticamente os campos de endereço

**Pré-condição:** Usuário autenticado na tela de cadastro de cliente

**Passos:**
1. Inserir nome completo
2. Inserir telefone
3. Inserir ZIP code válido (ex: 90210)
4. Verificar preenchimento automático

**Resultado esperado:** O sistema deve preencher automaticamente os campos de rua, cidade e estado.

**Resultado obtido:** Sistema realizou o preenchimento automático ao inserir ZIP code válido.

**Status:** PASS

---

### CT-037 — Preenchimento automático de endereço com CEP válido

**Módulo:** Cad. Cliente  
**Título:** Preenchimento automático de endereço com CEP válido

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro de cliente  
Quando ele insere um CEP válido brasileiro (ex: 41510-535)  
Então o sistema deve preencher automaticamente os campos de endereço

**Pré-condição:** Usuário autenticado na tela de cadastro de cliente

**Passos:**
1. Inserir nome completo
2. Inserir telefone
3. Inserir CEP válido brasileiro (41510-535)
4. Aguardar preenchimento automático

**Resultado esperado:** O sistema deve preencher automaticamente os campos de endereço ao inserir um CEP válido.

**Resultado obtido:** O sistema não realiza o preenchimento automático ao inserir um CEP válido, funcionando apenas com ZIP code.

**Status:** FAIL — BUG-007

---

### CT-038 — Ativar opção de cadastro de veículo

**Módulo:** Cad. Cliente  
**Título:** Ativar opção de cadastro de veículo

**Cenário (BDD):**  
Dado que o usuário está na tela de cadastro de cliente  
Quando ele ativa a opção de cadastro de veículo e preenche os dados  
Então o sistema deve permitir o cadastro do veículo associado ao cliente

**Pré-condição:** Usuário autenticado na tela de cadastro de cliente

**Passos:**
1. Preencher dados básicos do cliente
2. Ativar opção de cadastro de veículo
3. Preencher dados do veículo
4. Clicar em "Salvar Cadastro"

**Resultado esperado:** O sistema deve salvar o cliente junto com o veículo associado.

**Resultado obtido:** Sistema permitiu o cadastro do veículo associado ao cliente.

**Status:** PASS

---

## Cadastro de Agendamento

### CT-039 — Cadastro de agendamento com sucesso

**Módulo:** Novo Agendamento  
**Título:** Cadastro de agendamento com sucesso

**Cenário (BDD):**  
Dado que o usuário está na tela de novo agendamento  
Quando ele preenche os dados obrigatórios válidos e confirma o cadastro  
Então o sistema deve salvar o agendamento e exibir feedback visual de confirmação

**Pré-condição:** Usuário autenticado na tela de agendamento

**Passos:**
1. Selecionar cliente
2. Selecionar serviço
3. Definir data e horário
4. Confirmar o cadastro do agendamento

**Resultado esperado:** O sistema deve salvar o agendamento e exibir mensagem de confirmação.

**Resultado obtido:** O sistema cadastrou o agendamento, porém não apresentou feedback visual. O novo agendamento só foi exibido após atualização manual da página.

**Status:** FAIL — BUG-013

---

### CT-040 — Bloqueio de agendamento sem serviço cadastrado

**Módulo:** Novo Agendamento  
**Título:** Bloqueio de agendamento sem serviço cadastrado

**Cenário (BDD):**  
Dado que o usuário está na tela de novo agendamento  
Quando ele tenta criar um agendamento sem ter serviços cadastrados  
Então o sistema deve bloquear a criação e informar que não há serviços disponíveis

**Pré-condição:** Usuário autenticado na tela de agendamento

**Passos:**
1. Verificar ausência de serviços cadastrados
2. Tentar criar agendamento

**Resultado esperado:** O sistema deve bloquear o agendamento e exibir mensagem informativa.

**Resultado obtido:** Sistema exibiu mensagem bloqueando o agendamento.

**Status:** PASS

---

### CT-041 — Bloqueio de agendamento sem cliente com veículo cadastrado

**Módulo:** Novo Agendamento  
**Título:** Bloqueio de agendamento sem cliente com veículo cadastrado

**Cenário (BDD):**  
Dado que o usuário está na tela de novo agendamento  
Quando ele tenta criar um agendamento sem ter clientes com veículo cadastrado  
Então o sistema deve bloquear a criação e informar que não há clientes válidos

**Pré-condição:** Usuário autenticado na tela de agendamento

**Passos:**
1. Verificar ausência de clientes com veículo
2. Tentar criar agendamento

**Resultado esperado:** O sistema deve bloquear o agendamento e exibir mensagem informativa.

**Resultado obtido:** Sistema exibiu mensagem bloqueando o agendamento.

**Status:** PASS

---

### CT-042 — Cadastro de agendamento com campos obrigatórios vazios

**Módulo:** Novo Agendamento  
**Título:** Cadastro de agendamento com campos obrigatórios vazios

**Cenário (BDD):**  
Dado que o usuário está na tela de novo agendamento  
Quando ele clica em confirmar sem preencher os campos obrigatórios  
Então o sistema deve exibir as mensagens de validação para os campos obrigatórios

**Pré-condição:** Usuário autenticado na tela de agendamento

**Passos:**
1. Clicar em confirmar sem preencher dados
2. Observar mensagens de validação

**Resultado esperado:** O sistema deve exibir mensagens de validação para os campos obrigatórios.

**Resultado obtido:** Sistema exibiu as validações esperadas.

**Status:** PASS

---

## Resumo

| Seção | Total | PASS | FAIL |
|-------|-------|------|------|
| Login | 8 | 6 | 2 |
| Cadastro de Usuário | 11 | 9 | 2 |
| Home | 7 | 7 | 0 |
| Cadastro de Cliente | 12 | 5 | 7 |
| Cadastro de Agendamento | 4 | 4 | 0 |
| **TOTAL** | **42** | **31** | **11** |

**Taxa de sucesso: 74%**

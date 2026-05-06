# Melhorias — Detailer App

## Melhorias Identificadas

| ID | Módulo | Cenário | Descrição | Sugestão | Impacto |
|----|--------|---------|-----------|---------|---------|
| **MELHORIA-001** | Login | Definição de idioma da aplicação | A aplicação apresenta inconsistência de idioma entre os elementos da interface. Atualmente não há opção para o usuário definir o idioma desejado. | Adicionar opção de seleção de idioma (português/inglês) ou garantir padronização automática baseada na localização do usuário. | 1. Inconsistência de idioma pode confundir o usuário 2. Reduz a clareza das mensagens 3. Passa sensação de sistema não finalizado |
| **MELHORIA-002** | Login | Implementação de login com Google | O sistema apresenta opção de login com Google, porém a funcionalidade ainda não está implementada, exibindo a mensagem "Login com Google em breve!". | Implementar o fluxo completo de autenticação com Google ou ocultar o botão até que esteja disponível. | 1. Usuário pode tentar usar uma funcionalidade indisponível 2. Gera frustração ao não conseguir prosseguir 3. Pode causar percepção de sistema incompleto |
| **MELHORIA-003** | Cadastro | Validação de criação de conta | Após o cadastro, o sistema autentica o usuário imediatamente sem qualquer validação do email informado. | Implementar validação de email, como envio de email de confirmação de cadastro ou verificação de email antes de liberar acesso ao sistema. | 1. Pode permitir criação de contas com emails inválidos 2. Reduz segurança do sistema 3. Pode gerar problemas de recuperação de conta 4. Impacta confiabilidade dos dados |

## Análise

### Melhorias de UI/UX (1)
- **MELHORIA-001**: Padronização de idioma — melhoria cosmétic funcional

### Melhorias de Autenticação (2)
- **MELHORIA-002**: Login com Google — funcionalidade prometida mas não реализована
- **MELHORIA-003**: Validação de email no cadastro — melhoria de segurança

### Priorização

| Prioridade | Melhoria | Justificativa |
|------------|----------|---------------|
| **Alta** | MELHORIA-003 | Impacta segurança e integridade dos dados |
| **Média** | MELHORIA-002 | Funcionalidade visível ao usuário, gera frustração |
| **Baixa** | MELHORIA-001 | Melhoria de experiência, não bloqueia fluxo |

## Sugestões de Implementação

### MELHORIA-001: Definição de idioma
- Adicionar `DropdownButton` no topo da tela de login/settings
- Armazenar preferência em `SharedPreferences`
- Criar arquivos de tradução `pt.json` e `en.json`
- Aplicar em todas as strings da interface

### MELHORIA-002: Login com Google
- Configurar Firebase Auth com Google Provider
- Implementar `GoogleSignInProvider`
- Ocultar botão temporariamente ou adicionar snackbar "Em breve"

### MELHORIA-003: Validação de email
- Configurar Firebase Auth com email verification
- Adicionar lógica no backend (Cloud Function)
- Exibir banner na home até email verificado
- Bloquear login até verificação (opcional)

# Automações — [NOME DO PROJETO/USUÁRIO]

## 1. Descrição

Este diretório contém scripts de automação utilizados no projeto de QA para aumentar a eficiência e reduzir tarefas repetitivas.

---

## 2. Scripts Disponíveis

### 2.1 [Script 1]

**Arquivo:** `script1.py`  
**Linguagem:** Python  
**Descrição:** [Breve descrição do que o script faz]  
**Uso:**
```bash
python script1.py [argumentos]
```

**Argumentos:**
| Argumento | Descrição | Obrigatório |
|-----------|-----------|-------------|
| `--input` | Arquivo de entrada | Sim |
| `--output` | Arquivo de saída | Não |
| `--verbose` | Modo verboso | Não |

**Exemplo:**
```bash
python script1.py --input data.csv --output result.json --verbose
```

**Dependências:**
- `pandas`
- `openpyxl`

---

### 2.2 [Script 2]

**Arquivo:** `script2.py`  
**Linguagem:** Python  
**Descrição:** [Breve descrição]  
**Uso:**
```bash
python script2.py --mode [modo]
```

**Argumentos:**
| Argumento | Descrição | Obrigatório |
|-----------|-----------|-------------|
| `--mode` | Modo de execução (dev/prod) | Sim |

**Dependências:**
- `requests`
- `python-dotenv`

---

### 2.3 [Script 3]

**Arquivo:** `script3.sh`  
**Linguagem:** Bash  
**Descrição:** [Breve descrição]  
**Uso:**
```bash
./script3.sh [argumentos]
```

**Dependências:**
- `jq`
- `curl`

---

## 3. Configuração

### 3.1 Variáveis de Ambiente

Criar arquivo `.env` na raiz do diretório:

```bash
# API Keys
API_KEY=

# Configurações
DEBUG=true
MAX_RETRIES=3
TIMEOUT=30

# Paths
DATA_DIR=./data
OUTPUT_DIR=./output
```

### 3.2 Setup

```bash
# Criar ambiente virtual
python -m venv .venv
source .venv/Scripts/activate  # Windows: .venv\Scripts\activate

# Instalar dependências
pip install -r requirements.txt
```

---

## 4. Fluxos de Automação

### 4.1 Fluxo 1: [Nome do Fluxo]

```
[Step 1] → [Step 2] → [Step 3] → [Output]
   ↓          ↓          ↓           ↓
[Script1]  [Script2]  [Script3]  [Relatório]
```

**Passos:**
1. Executar `script1.py` com dados de entrada
2. Executar `script2.py` para processar resultados
3. Executar `script3.sh` para gerar relatório final

---

## 5. Manutenção

### 5.1 Logs

Logs são gerados em:
- `logs/script1.log`
- `logs/script2.log`
- `logs/script3.log`

### 5.2 Scheduling

Scripts podem ser agendados via cron (Linux) ou Task Scheduler (Windows):

**Exemplo cron (Linux):**
```cron
# Executar diariamente às 9h
0 9 * * * cd /path/to/automacoes && python script1.py >> logs/cron.log 2>&1
```

**Exemplo Task Scheduler (Windows):**
```batch
# Criar tarefa agendada
schtasks /create /tn "JobPilot Daily" /tr "python script1.py" /sc daily /st 09:00
```

---

## 6. Troubleshooting

### 6.1 Erro Comum: Módulo não encontrado

**Solução:**
```bash
pip install -r requirements.txt
```

### 6.2 Erro Comum: Permissão negada

**Solução:**
```bash
chmod +x script.sh
```

### 6.3 Erro Comum: Timeout

**Solução:** Aumentar `TIMEOUT` no `.env` ou verificar conexão de rede

---

## 7. Contribuição

Para adicionar novos scripts:

1. Criar script com nome descritivo (`YYYY_MM_DD_descricao.py`)
2. Adicionar docstring com descrição e uso
3. Adicionar logging
4. Atualizar este README
5. Adicionar requisitos em `requirements.txt`

---

## 8. Autora

**Ian Santos**  
**Data:** Maio 2026

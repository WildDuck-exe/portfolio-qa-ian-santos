# Automações — Portfólio QA

## Descrição

Scripts de automação utilizados para converter planilhas de QA (.xlsx) em documentação Markdown formatada, reduzindo trabalho manual e padronizando a saída dos documentos.

---

## Scripts Disponíveis

### gerar_docs_qa.py

**Arquivo:** `gerar_docs_qa.py`  
**Linguagem:** Python 3  
**Descrição:** Converte planilhas de casos de teste ou bugs (.xlsx) em documentação Markdown formatada com métricas automáticas.

**Uso:**
```bash
# Gerar casos de teste
python gerar_docs_qa.py --arquivo "planilha.xlsx" --projeto detailer-app

# Gerar bug report
python gerar_docs_qa.py --arquivo "bugs.xlsx" --projeto barber-os --tipo bugs

# Especificar saída customizada
python gerar_docs_qa.py --arquivo "dados.xlsx" --projeto klipper --saida output/resultado.md
```

**Argumentos:**

| Argumento | Descrição | Obrigatório |
|-----------|-----------|:-----------:|
| `--arquivo` | Caminho da planilha .xlsx | ✅ |
| `--projeto` | Nome da pasta do projeto | ✅ |
| `--tipo` | Tipo de documento: `casos` ou `bugs` (padrão: `casos`) | ❌ |
| `--saida` | Caminho de saída customizado | ❌ |

**Funcionalidades:**
- Lê planilhas .xlsx com headers na primeira linha
- Classifica automaticamente status (Passou/Falhou/Bloqueado)
- Calcula métricas consolidadas (taxa de aprovação, contagem por severidade)
- Emite avisos de qualidade (campos obrigatórios vazios)
- Ignora linhas completamente vazias

**Exemplo de saída:**
```
[INFO] 42 registros lidos de 'Casos de Testes.xlsx'

[OK] Gerado: projetos/detailer-app/casos-de-teste.md
     42 casos | ✅ 31 | ❌ 11 | ⚠️ 0 | ⬜ 0
     Taxa de aprovação: 74%
```

---

## Configuração

### Setup

```bash
# Criar ambiente virtual
python -m venv .venv

# Ativar (Windows)
.venv\Scripts\activate

# Ativar (Linux/Mac)
source .venv/bin/activate

# Instalar dependências
pip install -r requirements.txt
```

### Dependências

- `openpyxl` >= 3.1.0

---

## Próximas Automações (Planejado)

| Script | Descrição | Status |
|--------|-----------|--------|
| `gerar_docs_qa.py` | Conversor xlsx → Markdown | ✅ Implementado |
| `metricas_qa.py` | Gerador de métricas consolidadas | 📋 Planejado |
| `validar_links.py` | Verificador de links quebrados nos docs | 📋 Planejado |

---

**Autor:** Ian Santos  
**Data:** Maio 2026

# BarberOS — Análise de QA

## Contexto

O **BarberOS** é um sistema de gestão para barbearias utilizado **em produção por um cliente real**. O sistema abrange agendamento de clientes, registro de serviços, controle de comissões e exportação de dados financeiros.

Este é o case mais forte deste portfólio: os bugs documentados aqui são **reais**, encontrados em um ambiente de produção com dados reais de clientes e profissionais.

## Meu Papel

Atuei como **QA Analyst**, realizando testes exploratórios diretamente no ambiente de produção. Identifiquei bugs funcionais, documentei com análise de impacto no negócio e propus correções priorizadas por severidade.

## Objetivo

Validar a qualidade funcional do sistema BarberOS, com foco em:
- Cálculo correto de comissões
- Gestão de agendamentos
- Exportação de dados financeiros
- Integridade dos fluxos de trabalho

## Escopo Testado

| Módulo | Funcionalidades |
|--------|----------------|
| Agendamentos | Criação, visualização, fechamento |
| Comissões | Cálculo automático por profissional |
| Financeiro | Exportação de dados |
| Histórico | Visualização de agendamentos passados |

## Tipos de Teste Aplicados

- ✅ Teste funcional
- ✅ Teste exploratório
- ✅ Teste de regressão (planejado)

## Entregáveis de QA

| Documento | Descrição |
|-----------|-----------|
| [plano-de-testes.md](./plano-de-testes.md) | Estratégia, escopo e resultados |
| [bugs-reportados.md](./bugs-reportados.md) | 4 bugs documentados com impacto no negócio |
| [plano-regressao.md](./plano-regressao.md) | 4 cenários de regressão + 5 testes adicionais |
| [melhorias.md](./melhorias.md) | 7 melhorias propostas |
| [relatorio-final.md](./relatorio-final.md) | Resumo executivo com análise de impacto |

## Resultados

| Métrica | Valor |
|---------|-------|
| Cenários executados | 4 |
| Aprovados | 0 |
| Falhas | 4 |
| Bugs reportados | 4 |
| Melhorias propostas | 7 |
| **Taxa de falha** | **100%** |

> **Nenhum cenário de teste passou.** Todos os 4 bugs impactam diretamente a operação financeira do negócio.

## Principais Aprendizados

1. **Testar em produção com dados reais** revela bugs que não aparecem em homologação
2. **Bugs de cálculo financeiro** são os mais críticos em sistemas de gestão — impactam diretamente o bolso dos profissionais
3. **Documentação clara de bugs** com passos de reprodução, resultado esperado e impacto no negócio é essencial para priorização

## Próximos Passos

- Acompanhar correção dos 4 bugs reportados
- Executar plano de regressão após correções
- Expandir casos de teste para outros módulos
- Adicionar testes de API (endpoints de agendamento e comissão)

---

**Autor:** Ian Santos — Analista QA  
**Data:** Maio 2026

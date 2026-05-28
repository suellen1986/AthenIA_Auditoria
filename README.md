# 🦉 ATHENIA
### Inteligência Estratégica de Auditoria Interna

> *"Athena — deusa da sabedoria estratégica — nunca tomava decisões sem antes enxergar o campo de batalha por inteiro."*

**ATHENIA** é um sistema de inteligência de auditoria interna desenvolvido pela Coordenadoria de Auditoria Interna da **Petrobras**, com foco na análise trimestral de risco baseada em dados financeiros reais, plano estratégico e preocupações de stakeholders.

---

## 🎯 O Problema que Resolve

| Como era | Com ATHENIA |
|---|---|
| Auditor lê relatórios financeiros manualmente | Sistema extrai e estrutura automaticamente |
| Riscos avaliados de forma subjetiva | Riscos pontuados com base em dados reais do trimestre |
| PAA construído no Excel de forma manual | PAA gerado com IA, rastreável e justificado |
| MRE estática, atualizada esporadicamente | MRE viva, cruzada com achados do trimestre |
| Achados isolados sem contexto estratégico | Achados conectados ao risco corporativo e ao PN |
| Horas de trabalho para consolidar informações | Análise consolidada em minutos |

---

## ✨ Funcionalidades

### 📊 Dashboard de Análise Trimestral
- **Visão Geral** — Score global de risco por dimensão, KPIs financeiros, achados críticos
- **Desempenho Financeiro** — DRE, DFC, Balanço Patrimonial com variações 1T26 vs 1T25
- **Operacional** — Produção, E&P, refino, ramp-up de plataformas
- **Matriz de Riscos** — Mapa de calor Probabilidade × Impacto com 14 riscos mapeados
- **Tributário & Fiscal** — IE Exportação, ICMS, subvenções, histórico de recolhimento
- **Contencioso** — Provisões judiciais, ARO, adequação de disclosure
- **Estratégico** — Aderência ao PN 2026–2030, metas vs realizações
- **Webcast Stakeholders** — Mapeamento das preocupações de analistas e investidores

### 📘 PN 2026–2030 Integrado
- Cruzamento automático entre resultados do trimestre e metas do Plano de Negócios
- Análise de sensibilidade (Brent, câmbio, crack spread)
- Status dos grandes projetos do pré-sal
- Riscos não previstos no PN identificados no trimestre

### 📋 PAA — Plano Anual de Auditoria Gerado por IA
- **21 temas** gerados automaticamente com base nos riscos do trimestre
- Classificação por **COSO 2013** (5 componentes) e **IIA IPPF 2024** (Assurance / Advisory / Insight)
- Score de prioridade de 0 a 10 com justificativa rastreável
- Filtros por área auditada, componente COSO e nível de prioridade
- **Exportação CSV** para alimentar o processo atual de planejamento

---

## 🏛️ Universo Auditável

O ATHENIA cobre as **7 áreas auditadas** pela Coordenadoria:

| Área | Processos-Chave | Risco 1T26 |
|---|---|---|
| 🧾 **Tributário** | Planejamento fiscal, tributos diretos/indiretos, subvenções, compliance | 🔴 Crítico |
| ⚖️ **Jurídico** | Contencioso, ARO, contratos, compliance legal | 🔴 Crítico |
| 💰 **Finanças** | Dívida, CAPEX, dividendos, hedge, liquidez | 🔴 Crítico |
| 📊 **Desempenho** | KPIs, E&P, refino, gás, guidance | 🟠 Alto |
| ⚠️ **Riscos** | MRE, RCSA, mercado, liquidez, operacional, ESG | 🟠 Alto |
| 📒 **Contabilidade** | Fechamento, IFRS, notas, impairment, estoques | 🟡 Médio |
| 🏛️ **Governança** | CA, comitês, compliance, partes relacionadas, ESG | 🟡 Médio |

---

## 🔬 Fontes de Dados — 1T26

Todos os dados são extraídos de documentos públicos oficiais da Petrobras:

- 📄 **Demonstrações Financeiras Intermediárias** — 31/03/2026
- 📄 **Relatório Fiscal 1T26** — Visão Caixa
- 📄 **Relatório de Produção e Vendas 1T26**
- 🎙️ **Transcrição do Webcast** — 12/05/2026
- 📘 **Plano de Negócios 2026–2030** — Novembro/2025

---

## 🧭 Framework Metodológico

```
ATHENIA usa como espinha dorsal:

┌─────────────────────────────────────────────────┐
│  COSO ERM 2017 + COSO 2013                      │
│  ├── Ambiente de Controle                       │
│  ├── Avaliação de Risco                         │
│  ├── Atividades de Controle                     │
│  ├── Informação e Comunicação                   │
│  └── Monitoramento                              │
├─────────────────────────────────────────────────┤
│  IIA IPPF 2024                                  │
│  ├── Assurance (Garantia)                       │
│  ├── Advisory (Consultoria)                     │
│  └── Insight (Perspectiva)                      │
└─────────────────────────────────────────────────┘
```

---

## 🚀 Como Usar

O ATHENIA é um **arquivo HTML único** — não requer instalação, servidor ou dependências.

```
1. Faça o download do arquivo Index.html
2. Abra em qualquer navegador moderno (Chrome, Edge, Firefox)
3. Navegue pelas abas do dashboard
4. Use os filtros para segmentar por severidade e dimensão
5. Na aba PAA, filtre e exporte o CSV para seu processo atual
```

**Acesso online:** [https://suellen1986.github.io/AthenIA_Auditoria](https://suellen1986.github.io/AthenIA_Auditoria)

---

## 🗺️ Roadmap do Projeto

```
✅ v1.0 — MVP Dashboard (mai/2026)
   └── Análise 1T26 com 8 dimensões de risco

✅ v2.0 — ATHENIA com PAA + PN Integrado (mai/2026)
   └── 10 abas · 21 temas PAA · PN 2026-2030

⏳ v3.0 — MRE Interna + Mapa de Processos (próximo)
   └── Upload organograma + categorias MRE oficiais

⏳ v4.0 — Multi-trimestre (futuro)
   └── Comparativo histórico 1T25 → 4T25 → 1T26

⏳ v5.0 — Automação via API (futuro)
   └── Leitura automática dos PDFs a cada trimestre
   └── Integração com sistema interno da Petrobras
```

---

## 💡 Sobre o Projeto de Inovação

O ATHENIA nasceu da necessidade de **transformar a auditoria interna reativa em proativa**, conectando dados financeiros trimestrais diretamente ao planejamento de auditoria.

> *"A Petrobras produz R$72 bilhões em tributos por trimestre, opera o maior campo de águas profundas do mundo e tem R$83 bilhões em depósitos judiciais. A auditoria interna que monitora tudo isso merece uma ferramenta à altura."*

---

## 👩‍💼 Autoria

Desenvolvido pela **Coordenadoria de Auditoria Interna — Petrobras**

Coordenadora responsável pelo projeto de inovação.

---

## ⚠️ Disclaimer

Este dashboard é baseado **exclusivamente em documentos públicos** da Petrobras (ITR, relatórios operacionais, webcast de resultados e Plano de Negócios divulgados ao mercado). Nenhuma informação confidencial ou de uso restrito foi utilizada. Destinado a uso interno da Coordenadoria de Auditoria Interna.

---

<div align="center">

**🦉 ATHENIA — Inteligência Estratégica de Auditoria**

*"Enxergar o risco antes que ele se torne um problema."*

![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-Active-brightgreen)
![Framework](https://img.shields.io/badge/Framework-COSO%202013%20%2B%20IIA%20IPPF%202024-blue)
![Período](https://img.shields.io/badge/Período-1T26-yellow)
![Status](https://img.shields.io/badge/Status-MVP%20Ativo-success)

# 🦉 ATHENIA — README Técnico (Equipe de Desenvolvimento)

> Documento de transferência de conhecimento. Explica **como o ATHENIA foi construído por dentro**: o que a IA fez, quais skills e prompts foram usados, o que vem de Excel vs PDF, o que é estático vs dinâmico no painel, e como conectar tudo aos bancos de dados internos na próxima etapa.

Este README **não é** o institucional (esse explica o produto). Este é o documento de engenharia.

---

## 1. O que o ATHENIA é — tecnicamente

Hoje, o ATHENIA é **um único arquivo `index.html`** (~300 KB) — HTML + CSS + JavaScript vanilla, sem build, sem dependências externas além de Google Fonts. Roda em qualquer navegador e é servido como página estática pelo GitHub Pages.

**Implicação importante para vocês:** não há backend, não há banco, não há API. **Todos os dados estão embutidos no próprio HTML** (hardcoded no markup e em objetos JavaScript). O que parece um "sistema" é, na verdade, um protótipo de altíssima fidelidade. A Etapa 2 (seção 9) é o que o transforma em sistema de verdade.

```
┌─────────────────────────────────────────────┐
│  index.html (tudo dentro)                    │
│                                              │
│  <style>    → CSS (paleta, layout, temas)    │
│  <body>     → 12 abas com dados HARDCODED    │
│  <script>   → JS (navegação, filtros,        │
│               simulador, painel lateral)     │
└─────────────────────────────────────────────┘
         │
         ▼  servido estaticamente
   GitHub Pages (.nojekyll)
```

---

## 2. Como a análise foi feita (a verdade honesta)

**Não houve um pipeline automatizado nem um "prompt mágico" rodando por trás.** O ATHENIA foi produzido por **análise conversacional assistida por IA** (Claude): a coordenadora de auditoria descreveu, em linguagem natural, o que precisava; a IA leu os documentos oficiais, estruturou as análises aplicando frameworks de auditoria, calculou os indicadores e gerou o HTML.

Ou seja, em termos de engenharia:

- **Entrada:** documentos oficiais da Petrobras (PDFs + Excel) + instruções em linguagem natural.
- **Processamento:** raciocínio da IA sobre os documentos, aplicando COSO 2013, IIA IPPF 2024, análise fundamentalista e stress testing.
- **Saída:** o arquivo `index.html` com os dados já calculados e embutidos.

Quando alguém pergunta "qual prompt foi usado?", a resposta precisa é: *foi um trabalho conversacional guiado, não um prompt único*. **Porém**, a versão *sistematizável* desse trabalho — os prompts que rodariam se fosse automatizado — está documentada (ver seção 4).

---

## 3. Skills utilizadas

"Skills" aqui são bibliotecas de boas práticas que orientaram cada artefato produzido. Quatro foram usadas:

| Skill | Para que serviu | Onde aparece |
|-------|-----------------|--------------|
| **pdf-reading** | Estratégia de leitura dos PDFs (diagnóstico, rasterização, OCR) | Extração dos dados das Demonstrações Financeiras, Relatório Fiscal, Produção |
| **frontend-design** | Tokens de design, paleta, tipografia, elemento-assinatura | Construção do `index.html` na identidade Petrobras |
| **docx** | Geração de documentos Word profissionais (docx-js) | Documentação Técnica e Especificação de PI |
| **pptx** | Geração de PowerPoint (PptxGenJS), QA visual | Pitch deck para a área de Desenvolvimento |

Detalhamento de cada uma no documento `ATHENIA_Doc_Tecnica_Completa_v6.docx`, capítulo 2.

---

## 4. Os prompts (a versão sistematizável)

Embora a análise tenha sido conversacional, ela foi formalizada em **8 prompts de agentes especializados** — o desenho que a Etapa 2/Enterprise usaria para automatizar. Cada agente tem **escopo restrito a um domínio** para minimizar alucinação.

| Agente | Escopo | Fonte que lê | Saída |
|--------|--------|--------------|-------|
| **0 · Orquestrador** | Valida consistência entre agentes | Índice de todos | JSON de validação |
| **1 · Financeiro** | DRE, DFC, Balanço, 30+ indicadores | Excel (DRE/BP/DFC) | JSON de indicadores |
| **2 · Tributário** | IE, ICMS, ETR, diferidos | Excel + Relatório Fiscal | JSON de eventos fiscais |
| **3 · Jurídico** | Contencioso, provisões, ARO | PDF (NP 15, NP 16) | JSON de contencioso |
| **4 · Operacional** | Produção, segmentos, ativos | Excel + Relatório Produção | JSON operacional |
| **5 · Estratégico** | Aderência ao PN, desvios | PDF (PN) + saídas 1–4 | JSON de aderência |
| **6 · Webcast** | Q&A, compromissos, divergências | PDF (Transcrição) | JSON de stakeholders |
| **7 · PAA** | Gera temas COSO+IIA priorizados | Saídas dos agentes 1–6 | Lista priorizada |

**Princípio anti-alucinação aplicado em todos:** (1) escopo a um documento, (2) saída em JSON com schema fixo, (3) campo `fonte` obrigatório por valor, (4) instrução explícita "se não encontrar, responda 'não encontrado'".

Os prompts completos (system + user, com os schemas JSON) estão no `ATHENIA_Doc_Tecnica_Completa_v6.docx`, capítulo 4.

---

## 5. O que vem de Excel vs o que vem de PDF

Decisão de arquitetura da v7.0: **Excel é fonte primária**; PDF só para o que o Excel não tem.

### 📗 Vem do Excel (`Resultados_1T26 — Tabelas`) — 22 planilhas

> Fonte preferencial: dado estruturado, exato, com série histórica (1T26 + 4T25 + 1T25). Menor risco de erro de leitura.

- DRE completa, Balanço Patrimonial, DFC
- Receita líquida, CPV, Despesas operacionais, Resultado financeiro
- EBITDA Ajustado (oficial) e reconciliação
- Eventos exclusivos (lucro com/sem itens não recorrentes)
- Indicadores de endividamento (dívida em USD, taxa média, prazo médio)
- Liquidez e recursos de capital
- Resultado por segmento (E&P, RTC, G&EBC)
- Investimentos e principais projetos
- Brent médio, dólar médio (premissas reais do trimestre)

### 📕 Vem do PDF — somente o ausente no Excel

> Conteúdo qualitativo ou notas que não estão nas tabelas. PDFs das Demonstrações tinham **texto embutido corrompido** → exigiram **rasterização** (página → imagem → leitura visual via PyMuPDF a 150 DPI).

- **NP 15 e NP 16** (Contencioso e ARO) — ITR 31/03/2026
- **Transcrição do Webcast** — 12/05/2026 (Q&A dos analistas)
- **Plano de Negócios 2026–2030** (metas, premissas, projetos)

> ⚠️ Nota sobre o PN: o arquivo `.pdf` do PN era, na verdade, **texto UTF-8 com extensão errada**. Foi lido com `open(encoding='utf-8')`, não como PDF.

---

## 6. O que é ESTÁTICO vs DINÂMICO no painel

Crítico para vocês entenderem antes de mexer no código.

### 🔴 Estático (hardcoded) — quase tudo

Todos os **valores numéricos, tabelas, riscos, KPIs e textos** estão escritos diretamente no HTML. Não há fetch, não há cálculo em runtime para esses dados — eles foram calculados pela IA durante a construção e **colados** no markup.

```html
<!-- Exemplo: KPI hardcoded -->
<div class="kpi-value">123.686<span class="kpi-unit">MM R$</span></div>
<div class="kpi-delta down">▼ 7,3% vs 1T25</div>
```

```javascript
// Exemplo: dados do painel lateral de indicadores — objeto JS estático
const IND_DATA = {
  roic: { name:"ROIC", value:"13,7% a.a.", revela:"...", impacto:"...", ... },
  eva:  { name:"EVA",  value:"+R$15,2 bi", ... },
  // ... ~32 indicadores, todos pré-escritos
};
```

### 🟢 Dinâmico (roda no navegador) — só a interatividade

O JavaScript faz **interação com dados já presentes**, não busca nem persiste nada:

| Função JS | O que faz | Dinâmico? |
|-----------|-----------|-----------|
| `showTab(id)` | Troca a aba visível | Interação visual |
| `toggleFilter()` / `filterDimension()` | Filtra a matriz de risco por severidade/dimensão | Filtra DOM existente |
| `applyPAAFilters()` | Filtra os 21 temas do PAA | Filtra DOM existente |
| `exportPAACSV()` | Exporta o PAA visível para CSV | Gera arquivo client-side |
| `selectIndicator(key)` | Atualiza o painel lateral com `IND_DATA[key]` | Lê objeto JS estático |
| `showTree(type)` | Alterna entre as 4 árvores SVG | Mostra/oculta SVG |
| **`updateSim()` / `calcScenario()`** | **Recalcula FCO/ROIC/EVA/spread ao mover sliders** | **Cálculo real em runtime** |

> **O único cálculo verdadeiramente dinâmico é o simulador de sensibilidade** (`calcScenario`). Ele tem um modelo matemático em JS com as premissas reais do 1T26 (Brent 80,61, FX 5,26) e recalcula os resultados conforme o usuário move os sliders. Todo o resto é apresentação de dados pré-calculados.

```javascript
// O modelo do simulador — único cálculo em runtime
const BASE = { receita_anual:494744, capital_investido:799449, wacc:0.118,
               brent_base:80.61, fx_base:5.26, ie_base:22000,
               sens_brent:2000, sens_fx:12000, /* ... */ };

function calcScenario(brent, fx, ie, opexPct, prod) {
  const brentVar = (brent - BASE.brent_base) * BASE.sens_brent;
  // ... calcula receita, EBIT, NOPAT, ROIC, spread, EVA, FCO
  return { fco, roic, spread, eva, mgLiq };
}
```

---

## 7. Como os indicadores foram calculados

Os indicadores avançados não são chutes — seguem fórmulas de análise fundamentalista, com fonte rastreável. Exemplos (todos documentados no cap. 3 da Doc Técnica):

```python
# ROIC
nopat       = ebit * (1 - etr)              # etr = 33,3%
capital_inv = imobilizado + intangivel + nig
roic_anual  = (nopat / capital_inv) * 4
spread      = roic_anual - wacc             # wacc = 11,8% (ref. PN)
eva         = spread * capital_inv

# DuPont 5 fatores
roe = carga_trib × carga_fin × margem_ebit × giro_ativo × alavancagem

# NIG
nig = ACO - PCO   # negativa = passivos operacionais financiam o giro
```

Frameworks aplicados: **COSO 2013** (matriz de risco, PAA), **IIA IPPF 2024** (classificação Assurance/Advisory/Insight), **ISO 31000** (mapa de calor), **análise fundamentalista** (DuPont, ROIC, EVA), **stress testing** (sensibilidade).

---

## 8. Anatomia do arquivo

Para se localizarem no `index.html`:

```
<head>
  └─ <style>          paleta (CSS vars --pet-blue etc.), layout, componentes

<body>
  ├─ <header>         título + navegação das 12 abas (botões onclick=showTab)
  ├─ <main>
  │   ├─ #visao-geral        score, KPIs, seção Excel oficial, registro de riscos
  │   ├─ #financeiro         DRE, margens, DFC, balanço
  │   ├─ #operacional        produção, segmentos, riscos por ativo
  │   ├─ #risco-matriz       heatmap + tabela detalhada
  │   ├─ #tributario         eventos fiscais, histórico
  │   ├─ #contencioso        provisões, ARO (dados de PDF)
  │   ├─ #estrategico        aderência ao PN
  │   ├─ #webcast            Q&A (dados de PDF)
  │   ├─ #pn2026             KPIs do plano, projetos
  │   ├─ #paa2026            21 temas + filtros + export CSV
  │   ├─ #indicadores        tabelas + painel lateral (IND_DATA) + 4 árvores SVG
  │   └─ #sensibilidade      simulador (calcScenario) + heatmap + 5 cenários
  ├─ <footer>
  └─ <script>         todas as funções JS (seção 6)
</body>
```

---

## 9. Como conectar aos bancos de dados internos (Etapa 2)

Aqui está o que interessa para o futuro. Hoje os dados são hardcoded; a Etapa 2 os torna dinâmicos, vindos de banco. O caminho:

### 9.1 Inverter o fluxo de dados

```
HOJE:    dados calculados → colados no HTML → servido estático

ETAPA 2: fontes → ETL → PostgreSQL → API (FastAPI) → HTML consome via fetch
```

### 9.2 As três origens de dados a conectar

| Origem | Tipo | Como conectar |
|--------|------|---------------|
| **Dados contábeis do trimestre** | Externo, público | **API da CVM** (dados abertos — ITR/DFP em CSV/XBRL) |
| **Dados gerenciais** (EBITDA aj., segmentos, eventos exclusivos) | Externo, público | **Excels de Resultados** — download automatizado se URL estável, senão upload assistido |
| **MRE, apontamentos, controles, atribuições** | Interno, confidencial | **Conexão em leitura ao DW corporativo** que já alimenta o Power BI (SQL Server / Synapse / Fabric) |

### 9.3 Modelagem mínima do banco (resumo)

Especificação completa campo a campo no `ATHENIA_Especificacao_PI_Etapa2.docx`, capítulo 3. As tabelas:

```
temas_trimestrais        → o que o ATHENIA sugeriu em cada trimestre (série temporal)
indicadores_trimestrais  → indicadores calculados por trimestre (com thresholds KRI)
riscos_mre               → espelho (leitura) da Matriz de Riscos interna
apontamentos             → trabalhos anteriores + status (regularizado/pendente/vencido)
controles                → controles existentes por risco
areas_atribuicoes        → responsabilidades formais das áreas
vinculos_tema_historico  → liga cada tema ao histórico (o "enriquecimento")
```

### 9.4 As duas capacidades que a integração habilita

1. **Persistência trimestral:** gravar os temas de cada trimestre → série temporal → detectar riscos recorrentes, resolvidos ou que reaparecem. _Começar por aqui: não depende de acesso externo._
2. **Enriquecimento histórico:** para cada tema do trimestre, cruzar com `apontamentos` + `riscos_mre` + `controles` e responder automaticamente: já foi visto? foi regularizado? tem controle? está na MRE?

### 9.5 Onde o frontend muda

O `index.html` atual vira o template visual. Em vez de valores hardcoded, os elementos passam a ser populados por `fetch` à API:

```javascript
// HOJE (estático)
<div class="kpi-value">123.686</div>

// ETAPA 2 (dinâmico)
const dados = await fetch('/api/v1/indicadores/1T26').then(r => r.json());
document.querySelector('#kpi-receita').textContent = dados.receita_liquida;
```

### 9.6 Pontos de atenção para a Segurança da Informação

- Etapa 1 usa **só dados públicos**; Etapa 2 introduz **dados internos confidenciais** (papéis de trabalho, apontamentos).
- Definir: controle de acesso por perfil, criptografia, logs de acesso, e **o que pode trafegar pela API da Claude** (recomendação: só metadados estruturados e conteúdo público; dados confidenciais brutos ficam no ambiente interno).
- Isolar a camada de IA para que trocar de modelo seja mudar um endpoint, não reconstruir.

---

## 10. Artefatos do projeto (para consulta)

| Arquivo | Conteúdo |
|---------|----------|
| `index.html` | O dashboard v7.0 |
| `ATHENIA_Doc_Tecnica_Completa_v6.docx` | Skills (cap. 2), metodologia de cálculo (cap. 3), **prompts dos 8 agentes** (cap. 4), arquitetura enterprise (cap. 8) |
| `ATHENIA_Especificacao_PI_Etapa2.docx` | **Modelagem de dados** campo a campo, épicos do PI, fluxo de integração |
| `ATHENIA_PitchDeck_Desenvolvimento.pptx` | Apresentação executiva do projeto |

---

## 11. Resumo para quem vai pegar o projeto

- O ATHENIA hoje é um **HTML estático de alta fidelidade** — dados hardcoded, zero backend.
- A análise foi **conversacional assistida por IA**, formalizável nos **8 prompts** documentados.
- **Excel é a fonte primária**; PDF só para contencioso, webcast e PN.
- **Quase tudo é estático**; o único cálculo em runtime é o **simulador de sensibilidade**.
- A **Etapa 2** inverte o fluxo: fontes → banco → API → frontend, habilitando **memória trimestral** e **cruzamento com o histórico interno**.
- Comece pela **persistência** (não depende de ninguém); a integração com bases internas depende da liberação da SI.

---

<div align="center">

**🦉 ATHENIA — README Técnico**

Coordenadoria de Auditoria Interna · Petrobras · 2026

</div>

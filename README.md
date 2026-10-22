# 🏎️ Porsche Sales Intelligence — Executive BI Dashboard

Dashboard interativa de business intelligence corporativo desenvolvida para análise de performance comercial e operacional de vendas da **Porsche**. O projeto combina visualização de dados dinâmica, métricas de risco logístico e inteligência comercial automatizada em uma interface inspirada na identidade visual de alto padrão da marca.

🌐 **Dashboard Publicada (GitHub Pages):** [https://messias-cruz.github.io/dashboard-porsche-sales/](https://messias-cruz.github.io/dashboard-porsche-sales/)

---

## 📊 1. Perguntas de Negócio Escolhidas e Justificativas

Para proporcionar uma visão analítica profunda e direcionada à tomada de decisão C-Level, foram selecionadas três perguntas estratégicas e um painel de leitura executiva:

1. **CARD 1 | PERGUNTA 1 (Análise de Risco Logístico): *Qual a receita distribuída por Status de Entrega?***
   * **Por quê:** Em operações com veículos de altíssimo valor de luxo, o tempo de ciclo logístico e o represamento financeiro são críticos. Identificar o volume financeiro em status **Pendente**, **Em trânsito**, **Aguardando revisão** ou **Cancelado** permite mapear o capital em risco e tomar ações operacionais para proteger o fluxo de caixa da rede.
   * **Visual:** Gráfico de Rosca (*Doughnut Chart*) interativo com indicador dinâmico da % da receita em risco.

2. **CARD 2 | PERGUNTA 2 (Perfil de Uso vs. Depreciação): *Quilometragem Média vs. Preço Médio por Modelo***
   * **Por quê:** Avalia a relação de desvalorização entre diferentes linhas de veículos (ex: superesportivos da linha *911* mantêm valor mesmo com baixa rodagem, enquanto utilitários *Cayenne/Macan* apresentam maior impacto relativo pela quilometragem acumulada). Essa análise orienta políticas de precificação de seminovos e seminovos de frota.
   * **Visual:** Gráfico de Barras horizontais comparando Preço Médio vs. Quilometragem Média.

3. **CARD 3 | PERGUNTA 3 (Evolução por Ano do Veículo): *Faturamento e Volume de Vendas por Model Year (2020 - 2026)***
   * **Por quê:** Permite acompanhar a distribuição de receita entre veículos zero quilômetro/recentes (*2024–2026*) versus o giro de estoque de ano-modelo anterior (*2020–2023*), fornecendo insumos para o planejamento de reabastecimento junto às fábricas e concessionárias.
   * **Visual:** Gráfico Combo (*Combo Chart* com Colunas de Faturamento + Linha de Volume de Vendas).

4. **CARD 4 | LEITURA EXECUTIVA (Recomendações e Insights Automáticos):**
   * **Por quê:** Disponibiliza um resumo sintetizado (*Modelo Líder em Receita*, *Cidade com Maior Faturamento*, *Método de Pagamento Predominante* e *Estado Líder*) recalculado em tempo real conforme os filtros aplicados, permitindo diagnósticos ágeis sem necessidade de interpretação manual de múltiplos gráficos.

---

## 🛠️ 2. Tratamento da Base de Dados Antes da IA

Antes de enviar os dados para a geração da dashboard, a base bruta de vendas passou por um rigoroso processo de **higienização e sanitização automática** utilizando um agente em Python executado via Claude com o prompt:

> *"Crie um agente que leia o arquivo `8a213cd1-fc06-4f49-ae14-a057f014b6c1.xlsx` e aplique as regras do `Bootcamp DIO - Plano de Higienização de Vendas da Porsche.md` para gerar um novo arquivo tratado."*

### Principais ações realizadas no pré-processamento:
* **Padronização e Tratamento de Datas:** Identificação e marcação de 24 datas com formatos inconsistentes ou inexistentes no calendário (ex: `2024-02-30`, `April 31st, 2024`, `2024/15/07`) como `INVÁLIDO`. Datas de anos-modelo futuros (2026/2027) foram mantidas como válidas conforme o plano.
* **Conversão de Quilometragem:** Padronização de valores expressos em quilômetros para milhas (ex: `10.900 km` → `6.773 mi`).
* **Harmonização de Rótulos e Categorias:**
  * Correção de traduções literais (ex: *"Pimenta"* padronizado para *"Cayenne"*).
  * Unificação de duplicatas e variações de maiúsculas/minúsculas (*"Transferência bancária"* → *"Transferência Bancária"*).
  * Mapeamento de status equivalentes (*"pending review"* → *"Aguardando revisão"*).
* **Limpeza e Formatação Final:** Remoção de colunas redundantes e exportação da base final sanitizada com exatamente **100 registros integrais** no arquivo `Base Porsche_higienizado.xlsx`.

---

## 📝 3. O Prompt e a Evolução até a Versão Final

### Prompt Inicial (Estruturação Base)
```text
Atue como um desenvolvedor Front-end Senior e analista de BI especialista em UX/UI corporativo.

Crie uma dashboard interativa de alto padrão visual em um ÚNICO arquivo HTML completo (contendo CSS Tailwind via CDN e Chart.js via CDN embutidos).

💾 Base de Dados Oficial:
A base de dados completa (com exatamente 100 registros) está no arquivo Excel "Base Porsche_higienizado.xlsx" em anexo. 
Extraia TODOS os 100 registros sanitizados e insira-os no formato de array JSON dentro da variável `const salesData = [...]` no código JavaScript do HTML. 
NÃO resuma, NÃO omita registros e NÃO use reticências (...).

🎨 Diretrizes de Visual e UX/UI (Inspirado no Design Porsche Corporate):
1. Paleta de Cores: Fundo escuro metálico (#0B0D0E e #14171A), cards com fundo contrastante escuro (#1C2024), acentos em Dourado Porsche (#D4AF37 / #FFCC00) e rótulos/detalhes em Vermelho Esportivo (#D5001C).
2. Tipografia: 'Inter' via Google Fonts, estilo limpo e corporativo.
3. Card Hero / Destaque de Mídia Dinâmico:
   - Exiba no topo/lateral um painel com a foto HD do veículo selecionado (mídias CDN/Unsplash para as linhas 911, Taycan, Panamera, Cayenne, Macan, 718).
   - Defina como padrão a foto do '911 Turbo S'.
   - Inclua tratamento HTML onerror com fallback.
   - Mostre abaixo da imagem as métricas médias do modelo filtrado (Preço Médio e Quilometragem Média).

📐 Estrutura do Layout (Grid Responsivo):
1. Cabeçalho Executivo: Título "Porsche Sales Intelligence" e Badge de integridade: "100 Vendas Sanitizadas".
2. Barra Superior de Filtros Interativos: Agrupamento/Modelo, Estado, Cidade, Ano, Método de Pagamento e Status de Entrega.
3. Cards de KPIs Principais: Receita Total, Total de Unidades Vendidas, Ticket Médio Geral, Taxa de Conclusão de Entrega (%).
4. Grid de Gráficos e Perguntas de Negócio (2x2):
   - CARD 1 | PERGUNTA 1: Receita por Status de Entrega (Donut Chart).
   - CARD 2 | PERGUNTA 2: Quilometragem Média vs. Preço Médio por Modelo (Bar Chart).
   - CARD 3 | PERGUNTA 3: Faturamento e Volume por Model Year (Combo Chart).
   - CARD 4 | RECOMENDAÇÕES: Leitura Executiva automatizada com 4 sub-cards.

⚙️ Lógica JavaScript Exigida:
- Função central `updateDashboard()` para escutar mudanças nos dropdowns, recalcular KPIs, atualizar gráficos com `chart.update()` e alterar dinamicamente os textos dos cards da Leitura Executiva.

O que mudou até a versão final:
Agrupamento por Famílias de Veículos: Criação de uma lógica no JavaScript (get_family()) para agrupar os 40 modelos específicos nas 6 famílias principais (911, Taycan, Panamera, Cayenne, Macan e 718), permitindo filtragem individual ou combinada.

Correção da Integridade do Contador (97 vs. 100 Registros): Identificação e correção de um comportamento assíncrono na renderização inicial dos selects de cidade que aplicava um filtro residual na carga da página. A função populateCityDropdown() foi ajustada para garantir a leitura integral dos 100 de 100 registros sanitizados.

Mídia Dinâmica Sincronizada (Hero Spotlight): Aprimoramento da troca dinâmica das imagens CDN com base no modelo selecionado e tratamento com imagem fallback institucional Porsche via onerror.

🤖 4. Ferramentas e IAs Utilizadas
ChatGPT (Canvas): Utilizado inicialmente para exploração das primeiras versões do código. Contudo, devido a limitações de ambiente no plano gratuito (ausência de suporte para criação de agentes personalizados e limitações de contexto), o desenvolvimento foi migrado.

Google Gemini: Utilizado para o desenvolvimento completo do front-end corporativo, estruturação dos gráficos no Chart.js, estilização responsiva com Tailwind CSS, refatoração da lógica de atualização do dashboard e garantia de integridade dos 100 registros em JavaScript.

Claude: Utilizado com agente Python dedicado para a etapa prévia de higienização e sanitização da planilha Excel oficial.

🚀 5. Como Executar o Projeto Localmente
Clone o repositório público:

Bash
git clone [https://github.com/messias-cruz/dashboard-porsche-sales.git](https://github.com/messias-cruz/dashboard-porsche-sales.git)
Navegue até o diretório do projeto:

Bash
cd dashboard-porsche-sales
Abra o arquivo index.html em qualquer navegador web moderno. Não é necessário instalar dependências nem subir um servidor local, pois todas as bibliotecas (Tailwind CSS e Chart.js) são carregadas via CDN.

💻 Tecnologias Utilizadas
HTML5 / JavaScript (ES6+)

Tailwind CSS (via CDN)

Chart.js (via CDN)

GitHub Pages (Hospedagem)


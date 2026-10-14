# 📊 Dashboard Interativo de Salários na Área de Dados

Dashboard interativo em Python com **Streamlit** para explorar dados salariais de profissionais da área de dados. A aplicação lê um CSV remoto com `pandas`, oferece filtros na barra lateral, exibe KPIs, gráficos com `Plotly` e a tabela detalhada dos registros filtrados.

> Este README descreve o código efetivamente implementado em `app.py` nesta revisão.

## Demonstração online

Link informado no projeto original:

- <https://dashboard-interativo-com-python.streamlit.app/>

Disponibilidade não verificada nesta auditoria (em 14/10/2026, a checagem automatizada retornou timeout via leitor web e respostas HTTP `303` de redirecionamento de autenticação do Streamlit, sem confirmação de renderização do app). Use a execução local abaixo como referência confiável.

Repositório:

- <https://github.com/marllonpanisset/dashboard-interativo-com-python>

## Contexto e finalidade

Projeto de análise e visualização de dados salariais. Finalidade:

- Filtrar registros por ano, senioridade, tipo de contrato e tamanho da empresa.
- Resumir salário médio, salário máximo, volume de registros e cargo mais frequente.
- Comparar cargos, distribuição salarial, regime de trabalho e salário médio de `Data Scientist` por país.

O escopo é exploratório. Não há modelagem estatística, predição ou testes automatizados neste repositório.

## Tecnologias

| Tecnologia | Versão fixada | Função no projeto |
| --- | --- | --- |
| Python | ver `.devcontainer` (imagem Python 3.11) | Linguagem da aplicação |
| Streamlit | `1.44.1` | Interface, filtros laterais, KPIs, tabela |
| pandas | `2.2.3` | Leitura do CSV remoto, filtragem com `isin`, agregações (`mean`, `max`, `mode`, `groupby`) |
| Plotly Express | `5.24.1` | Gráficos `bar`, `histogram`, `pie` e `choropleth` |
| Dev Container | imagem `mcr.microsoft.com/devcontainers/python:1-3.11-bullseye` | Ambiente reproduzível, porta `8501` |

Configuração da página em `app.py`:

- `st.set_page_config(page_title="Dashboard de Salários na Área de Dados", page_icon="📊", layout="wide")`
- Título principal: `🎲 Dashboard de Análise de Salários na Área de Dados`

## Origem dos dados

Fonte única, remota, lida a cada execução sem cache:

```python
pd.read_csv("https://raw.githubusercontent.com/marllonpanisset/Imersao-Dados-com-Python-Alura/refs/heads/main/Aula_04/dados-imersao-final.csv")
```

Repositório de origem do dado: `marllonpanisset/Imersao-Dados-com-Python-Alura`, pasta `Aula_04`, arquivo `dados-imersao-final.csv`.

**Dependência de internet obrigatória.** Sem conexão, ou se a URL sair do ar, o app falha no carregamento — não há tratamento de exceção nem arquivo local de fallback.

Colunas utilizadas pelo código: `ano`, `senioridade`, `contrato`, `cargo`, `usd`, `remoto`, `tamanho_empresa`, `residencia_iso3`. O CSV completo também contém `salario`, `moeda`, `residencia` e `empresa`.

Valores observados na fonte (amostra de até 20.001 linhas em 14/10/2026):

- `ano`: `2022`, `2024`, `2025`
- `senioridade`: `executivo`, `junior`, `pleno`, `senior`
- `contrato`: `contrato`, `integral`, `parcial`
- `tamanho_empresa`: `grande`, `media`
- `remoto`: `hibrido`, `presencial`, `remoto`
- Cargos mais frequentes na amostra: `Software Engineer`, `Engineer`, `Data Engineer`, `Data Scientist`, `Data Analyst`

## Funcionalidades implementadas

- Barra lateral `🔍 Filtros` com 4 `st.sidebar.multiselect`, todos com padrão = todas as opções selecionadas:
  - `Ano` a partir de `df['ano']`
  - `Senioridade` a partir de `df['senioridade']`
  - `Tipo de Contrato` a partir de `df['contrato']`
  - `Tamanho da Empresa` a partir de `df['tamanho_empresa']`
- Filtragem combinada com `&` e `isin` em `df_filtrado`.
- Seção `Métricas gerais (Salário anual em USD)`.
- Seção `Gráficos` com 4 visualizações em layout 2x2.
- Seção `Dados Detalhados` com `st.dataframe(df_filtrado)` integral, sem paginação customizada.

Não há: exportação de CSV, autenticação, variáveis de ambiente, scripts auxiliares ou salvamento de estado.

## Indicadores (KPIs)

Calculados sobre `df_filtrado` quando não vazio (`app.py:51-55`):

- Salário médio: `df_filtrado['usd'].mean()`
- Salário máximo: `df_filtrado['usd'].max()`
- Total de registros: `df_filtrado.shape[0]`
- Cargo mais frequente: `df_filtrado["cargo"].mode()[0]`

Exibidos em 4 colunas com `st.metric`. Quando os filtros não retornam registros, o app exibe `st.warning` e mostra `$0`, `$0`, `0` e `—`, sem calcular estatísticas sobre o DataFrame vazio.

## Visualizações

Todas usam Plotly Express e só renderizam se `df_filtrado` não estiver vazio; caso contrário exibem `st.warning`:

1. `Top 10 cargos por salário médio` — `px.bar` horizontal a partir de `groupby('cargo')['usd'].mean().nlargest(10)`.
2. `Distribuição de salários anuais` — `px.histogram(x='usd', nbins=30)`.
3. `Proporção dos tipos de trabalho` — `px.pie` tipo donuts (`hole=0.5`) a partir de `df_filtrado['remoto'].value_counts()`, com `textinfo='percent+label'`.
4. `Salário médio de Cientista de Dados por país` — `px.choropleth(locations='residencia_iso3', color='usd', color_continuous_scale='rdylgn')`, calculado **somente** sobre `df_filtrado[df_filtrado['cargo'] == 'Data Scientist']` agrupado por `residencia_iso3`.

Observação: apesar do README anterior citar “mapa de calor”, o implementado é um mapa coroplético (`choropleth`), não um heatmap.

## Estrutura do repositório

```text
.
├── app.py                            # Aplicação Streamlit (arquivo principal)
├── requirements.txt                  # pandas==2.2.3, streamlit==1.44.1, plotly==5.24.1
├── readme.md                         # Esta documentação (nome em minúsculas no repo)
└── .devcontainer/devcontainer.json   # Dev Container Python 3.11, forward da porta 8501
```

Não há `LICENSE`, `AGENTS.md`, testes, `packages.txt` ou pasta de dados locais. O comando `updateContentCommand` do devcontainer cita `packages.txt` condicionalmente, mas o arquivo não existe no repositório.

## Requisitos para execução local

- Linux com Python 3 e `pip` disponíveis (referência: imagem Python 3.11 do devcontainer).
- Módulo `venv` (em geral já incluso; em algumas distros via pacote `python3-venv`).
- Conexão com a internet (leitura do CSV remoto).
- Navegador para `http://localhost:8501`.
- Dependências listadas em `requirements.txt`.

## Como executar localmente

No diretório raiz do repositório:

```bash
python3 --version
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
streamlit run app.py
```

Em Windows (apenas referência, comandos principais acima são para Linux):

```powershell
.venv\Scripts\Activate
```

### Como acessar

Após `streamlit run app.py`, abra:

- <http://localhost:8501>

O terminal do Streamlit também exibe os endereços `Local URL` e `Network URL`. Para encerrar, use `Ctrl+C` e depois `deactivate` para sair do ambiente virtual.

## Como reproduzir e testar os filtros

1. Com todos os filtros selecionados (padrão), confirme 4 KPIs, 4 gráficos e a tabela cheia.
2. Na barra lateral, remova um valor por vez (ex.: deixe só `Ano = 2025`) e observe KPIs e gráficos atualizarem.
3. Combine `Senioridade = senior` + `Tipo de Contrato = integral` e confira o Top 10 e o histograma.
4. Filtre um cenário sem `Data Scientist` (se possível com os dados atuais) e observe que o mapa por país fica vazio, enquanto os demais gráficos ainda mostram dados — comportamento esperado pelo filtro `cargo == 'Data Scientist'`.
5. **Caso de filtro sem nenhum registro** (ex.: desmarcar todas as opções de qualquer filtro): o app exibe o aviso “Nenhum registro encontrado para os filtros selecionados”, mostra as métricas como `$0`, `$0`, `0` e `—`, exibe os avisos de “Nenhum dado” nos 4 gráficos e uma tabela vazia.

## Publicação no Streamlit Community Cloud (informação geral, opcional)

Esta seção é orientação genérica, **não** o estado atual do projeto. O único link de deploy conhecido é o listado em [Demonstração online](#demonstração-online), sem disponibilidade garantida.

Para publicar este repositório específico:

1. Suba esta branch `main` com `app.py` e `requirements.txt` para um repositório público no GitHub.
2. Acesse o Streamlit Community Cloud e entre com o GitHub.
3. Crie um novo app apontando para o repositório, branch `main` e arquivo principal `app.py`.
4. Faça o deploy e use o link público gerado.

Documentação oficial: <https://docs.streamlit.io/deploy/streamlit-community-cloud>

## Limitações conhecidas

- **Filtros sem resultados (`app.py:51-61`):** comportamento tratado. O ramo `else` exibe `st.warning`, inicializa `salario_medio = 0`, `salario_maximo = 0`, `total_registros = 0` e `cargo_mais_frequente = "—"` — valores compatíveis com as formatações dos `st.metric` — e não executa `mean`/`max`/`mode` sobre o DataFrame vazio. Os gráficos mantêm os avisos de “Nenhum dado”.
- **Sem cache e sem tratamento de rede:** `pd.read_csv` remoto executa a cada interação, sem `@st.cache_data` e sem `try/except`.
- **Mapa restrito a `Data Scientist`:** mesmo com outros cargos filtrados, o choropleth só reflete esse cargo. Se não houver `Data Scientist` no filtro, o mapa fica vazio sem o aviso padronizado dos demais gráficos.
- **Sem testes, sem lint configurado e sem licença definida.**
- **Nomes de arquivo com case sensível:** o arquivo é `readme.md` (minúsculas); o devcontainer referencia `README.md` em `openFiles`.

## Créditos

- Código e deploy original: [marllonpanisset](https://github.com/marllonpanisset) (`dashboard-interativo-com-python`).
- Dados: `Imersao-Dados-com-Python-Alura` — <https://github.com/marllonpanisset/Imersao-Dados-com-Python-Alura>.
- Bibliotecas: Streamlit, pandas e Plotly (ver `requirements.txt`).

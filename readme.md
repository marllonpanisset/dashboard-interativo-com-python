# 📊 Dashboard Interativo de Salários na Área de Cientista de Dados

Este projeto consiste em um dashboard interativo desenvolvido em python com a biblioteca **Streamlit**, que permite explorar dados salariais de profissionais da área de dados ao redor do mundo. A aplicação oferece filtros dinâmicos, visualizações gráficas e métricas principais para facilitar a análise e a compreensão dos dados.

---

## 🚀 Sobre o Projeto

O dashboard foi construído com foco em usabilidade e clareza visual. A partir de um conjunto de dados públicos, foi possível aplicar técnicas de filtragem, agregação e visualização para extrair insights relevantes. O projeto foi dividido em etapas:

- **Configuração da interface**: Definição de título, ícone e layout com `st.set_page_config`.
- **Carregamento de dados**: Leitura de um arquivo CSV hospedado remotamente com `pandas`.
- **Barra lateral de filtros**: Filtros interativos para ano, senioridade, tipo de contrato e tamanho da empresa.
- **Métricas principais (KPIs)**: Cálculo e exibição de indicadores como salário médio, salário máximo, total de registros e cargo mais frequente.
- **Visualizações com Plotly**:
  - Gráfico de barras com os 10 cargos mais bem pagos.
  - Histograma da distribuição salarial.
  - Gráfico de pizza sobre tipos de trabalho (remoto, presencial, híbrido).
  - Mapa de calor com salários médios por país para Cientistas de Dados.
- **Tabela de dados detalhados**: Exibição dos dados filtrados em formato tabular.

---

## 📚 O que foi aprendido

Durante o desenvolvimento do projeto, foram explorados diversos conceitos fundamentais:

- Manipulação de dados com `pandas`
- Criação de interfaces interativas com `Streamlit`
- Visualização de dados com `Plotly`
- Boas práticas de organização de código e experiência do usuário
- Aplicação de filtros dinâmicos e métricas agregadas

---

## 🛠️ Como rodar o projeto localmente


### 1. Criar o ambiente virtual

1. Instale o python em seu sistema operacional.

2. Crie o ambiente virtual:

```bash
python3 -m venv .venv
```

3. Ativar o ambiente virtual em Windows:

```bash
.venv\Scripts\Activate
```

4. Ativar o ambiente virtual em MAC/LINUX:
```bash
source .venv/bin/activate
```

5. Instalar as bibliotecas necessárias:

```bash
pip install -r requirements.txt
```

6. Rodar o Projeto Local:

```bash
streamlit run app.py
```

## ☁️ Como realizar o deploy no Streamlit Cloud

Você pode publicar seu dashboard gratuitamente usando o [Streamlit Cloud](https://streamlit.io/cloud). Siga os passos abaixo:

### 1. Suba o projeto para um repositório público no GitHub

Certifique-se de que os seguintes arquivos estejam presentes no repositório:

- `app.py` — arquivo principal da aplicação Streamlit
- `requirements.txt` — lista de dependências do projeto
- `README.md` — documentação do projeto (opcional, mas recomendado)

> 💡 Dica: mantenha o nome do arquivo principal como `app.py` para facilitar o deploy.

---

### 2. Acesse o site do Streamlit Cloud

[https://streamlit.io/cloud](https://streamlit.io/cloud)

---

### 3. Faça login com sua conta do GitHub

Clique em **"Sign in with GitHub"** e autorize o acesso do Streamlit à sua conta.

---

### 4. Crie um novo app

Após o login:

- Clique em **"New app"**
- Selecione o repositório onde está o projeto
- Escolha a branch correta (ex: `main`)
- Informe o caminho do arquivo principal (ex: `app.py`)

---

### 5. Clique em **"Deploy"**

O Streamlit iniciará o deploy automaticamente e abrirá o dashboard em uma nova aba do navegador.

> 🔗 Você receberá um link público que pode ser compartilhado com outras pessoas.

---

### ✅ Pronto!

Seu dashboard está publicado e acessível online. Sempre que fizer alterações no repositório, o Streamlit pode atualizar automaticamente a aplicação.

> ⚙️ Se quiser configurar variáveis de ambiente, autenticação ou personalizar o deploy, explore as opções avançadas no painel do Streamlit Cloud.


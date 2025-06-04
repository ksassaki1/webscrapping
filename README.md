# 🕸️ **Projeto de Web Scraping**

Este projeto demonstra como extrair automaticamente citações do site http://quotes.toscrape.com utilizando **Python**, `requests` e `BeautifulSoup`. Ao final, os dados coletados são organizados em um DataFrame do `pandas` e exportados para um arquivo CSV.

---

## 🎯 **Objetivo**
- Desenvolver um script simples e replicável para **raspar** citações (texto e autor) de uma página web estática.  
- Demonstrar etapas de **coleta**, **parsing** e **manipulação** de dados em Python, culminando na geração de um arquivo CSV com as informações.

---

## 🛠 **Tecnologias e Ferramentas Usadas**
- **Linguagem:** Python  
- **Bibliotecas:**
  - `requests`: Para enviar requisições HTTP e obter o HTML bruto da página.  
  - `BeautifulSoup` (do pacote `bs4`): Para analisar o DOM e extrair elementos desejados.  
  - `pandas`: Para estruturar e manipular os dados coletados em um DataFrame e salvá-los em CSV.  
  - `Jupyter Notebook`: Ambiente interativo para desenvolvimento e demonstração do fluxo de trabalho.  
  - `Conda` ou `venv`: Gerenciamento do ambiente de dependências.

---

## 📂 **Estrutura do Projeto**
### **Arquivos e Diretórios**
- **`webscraping_example.ipynb`**: Notebook principal contendo todas as etapas de scraping, análise e exportação.  
- **`quotes.csv`**: Arquivo CSV gerado contendo as citações extraídas (texto, autor e tamanho).  
- **`README.md`**: Descrição completa do projeto, instruções e metodologia.

```
    .
    ├── webscraping_example.ipynb
    ├── quotes.csv
    └── README.md
```
---

## 🧠 **Métodos Implementados**
- **Importação de Bibliotecas**:  
  - Início do notebook com `import requests`, `from bs4 import BeautifulSoup` e `import pandas as pd` para preparar o ambiente de scraping e análise.

- **Requisição HTTP e Validação**:  
  - Uso de:
    ```python
    url = 'http://quotes.toscrape.com'
    response = requests.get(url)
    ```
  - Verificação de `response.status_code == 200` para garantir que a página foi acessada corretamente.

- **Parsing do HTML com BeautifulSoup**:  
  - Criação de:
    ```python
    soup = BeautifulSoup(response.text, 'html.parser')
    ```
    para construir a árvore DOM.  
  - Seleção de todos os elementos `<div class="quote">` por meio de:
    ```python
    quotes = soup.find_all('div', class_='quote')
    ```
    ou
    ```python
    quotes = soup.select('div.quote')
    ```

- **Extração dos Dados (Texto e Autor)**:  
  - Iteração sobre cada bloco de citação extraído, usando:
    ```python
    data = []
    for quote in quotes:
        text = quote.find('span', class_='text').get_text()
        author = quote.find('small', class_='author').get_text()
        data.append({'quote': text, 'author': author})
    ```
  - Armazenamento em lista de dicionários, seguida de conversão em DataFrame via:
    ```python
    df = pd.DataFrame(data)
    ```

- **Manipulação e Exploração Rápida com pandas**:  
  - Exibição das primeiras linhas:
    ```python
    df.head()
    ```
  - Verificação de estrutura e tipos:
    ```python
    df.info()
    ```
  - Contagem de citações por autor:
    ```python
    df['author'].value_counts()
    ```
  - Filtragem por palavra-chave, por exemplo:
    ```python
    df[df['quote'].str.contains('truth', case=False)]
    ```
  - Adição de coluna `length` com o tamanho de cada citação:
    ```python
    df['length'] = df['quote'].str.len()
    df.describe()
    ```
  - Ordenação por tamanho de citação:
    ```python
    df.sort_values(by='length', ascending=False)
    ```

- **Exportação para CSV**:  
  - Salvamento completo em:
    ```python
    df.to_csv('quotes.csv', index=False)
    ```

---

## 📊 **Resultados Obtidos**
Após a execução do notebook, foram coletadas todas as citações exibidas na página inicial de http://quotes.toscrape.com (totalizando 10 registros). Cada registro contém as colunas **quote** (texto da frase), **author** (nome do autor) e **length** (número de caracteres do texto).  

**Conclusão Geral**: O script demonstrou capacidade de coletar e estruturar dados de forma rápida, além de permitir manipulação básica dos resultados com pandas para análises simples de frequência e tamanho das citações.



## 🚀 **Próximos Passos**
- **Paginação Automática**: Implementar lógica de iteração por múltiplas páginas verificando o link “next”.  
- **Tratamento de Erros e Retries**: Adicionar blocos `try/except` e estratégia de backoff para lidar com falhas de conexão ou status diferentes de 200.  
- **Rotação de User-Agent e Uso de Proxies**: Incluir cabeçalhos dinâmicos e proxies para evitar bloqueios em sites mais restritivos.  
- **Extração de Conteúdo Dinâmico**: Introduzir Selenium ou Playwright para capturar dados renderizados por JavaScript.  
- **Armazenamento em Banco de Dados**: Em vez de CSV, persistir os resultados em SQLite, PostgreSQL ou outro banco, usando `df.to_sql()`.

---

## 👤 **Autor**
Guilherme Koiti Tanaka Sassaki  
[LinkedIn](https://www.linkedin.com/in/guilherme-sassaki-10b81ba7/)


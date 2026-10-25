# Preditor de Pit Stop 🏎️⚙️

> Um projeto de Data Lakehouse e Machine Learning de ponta a ponta que utiliza PySpark, Arquitetura Medalhão e integração de APIs para prever o momento ideal de um pit stop na Fórmula 1.

## 📖 Sobre o Projeto

O **Preditor de Pit Stop** une processamento distribuído e inteligência artificial para resolver um dos maiores desafios estratégicos do automobilismo: a tomada de decisão em tempo real. 

O projeto estrutura um Data Lakehouse para integrar dados históricos da F1, telemetria, uso de pneus e condições meteorológicas. A partir dessa base consolidada, treinamos modelos de classificação em PySpark para avaliar a degradação, o clima e o comportamento adversário, prevendo a necessidade de um pit stop volta a volta.

## 🏗️ Arquitetura de Dados (Medalhão)

A pipeline foi construída utilizando o **PySpark** e o formato **Delta Lake**, dividida em três camadas principais:

*   🥉 **Camada Bronze (Raw):** Ingestão de dados brutos em seu formato original.
    *   CSVs do [Dataset Histórico do Kaggle](https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020/data).
    *   JSONs/Parquets extraídos via [FastF1 API](https://docs.fastf1.dev/) (Telemetria e Pneus).
    *   Dados climáticos de APIs externas.
*   🥈 **Camada Silver (Cleansed & Conformed):** Limpeza, tipagem, tratamento de nulos e cruzamento (Joins) entre a base do Kaggle e a telemetria do FastF1. Onde os dados se tornam estruturados e confiáveis.
*   🥇 **Camada Gold (Feature Store):** Tabela final focada em *Feature Engineering* para o modelo de ML. Contém cálculos complexos usando *Window Functions*, como:
    *   Média móvel de degradação de tempo nas últimas 3 voltas.
    *   Idade do composto do pneu atual.
    *   Indicador de "Undercut" (piloto adversário parou nos boxes).

## 🧠 Machine Learning

O objetivo preditivo é classificar, a cada volta completada, se o piloto **deve ir para os boxes na próxima volta (1 = Sim, 0 = Não)**.

Devido ao forte desbalanceamento de classes (existem muito mais voltas sem paradas do que com paradas), o projeto foca nas seguintes abordagens:
*   **Modelos Avaliados:** Logistic Regression, Random Forest Classifier e Gradient-Boosted Trees (GBT).
*   **Métricas de Avaliação:** F1-Score (focado na classe minoritária) e Precision-Recall AUC (PR-AUC), descartando a acurácia simples.

## 🛠️ Tecnologias Utilizadas

*   **Linguagens:** Python, SQL
*   **Big Data & Engenharia:** Apache Spark (PySpark), Delta Lake, Databricks / Docker
*   **Machine Learning:** Spark MLlib, MLflow (para rastreamento de experimentos)
*   **Fontes de Dados:** Kaggle, FastF1, APIs Meteorológicas

## 📂 Estrutura do Repositório (Em construção)

```text
├── data/
│   ├── bronze/     # Dados brutos ingeridos
│   ├── silver/     # Dados limpos e combinados
│   └── gold/       # Feature table pronta para ML
├── notebooks/
│   ├── 01_ingestao_bronze.ipynb
│   ├── 02_etl_silver.ipynb
│   ├── 03_features_gold.ipynb
│   └── 04_machine_learning.ipynb
├── src/            # Scripts Python reutilizáveis (extração API, funções)
└── README.md
```

## 🚀 Como Executar

*(Instruções de como subir o ambiente local via Docker ou importar os notebooks no Databricks Community Edition serão adicionadas em breve).*

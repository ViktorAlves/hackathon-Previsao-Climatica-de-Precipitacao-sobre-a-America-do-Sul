# 🌧️ Previsão de Precipitação na América do Sul — Hackathon WORCAP 2026 (INPE)

<p align="center">
  <img src="https://img.shields.io/badge/Python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54" alt="Python" />
  <img src="https://img.shields.io/badge/TensorFlow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white" alt="TensorFlow" />
  <img src="https://img.shields.io/badge/Kaggle-20BEFF?style=for-the-badge&logo=Kaggle&logoColor=white" alt="Kaggle" />
  <img src="https://img.shields.io/badge/MackIA-Mackenzie-blue?style=for-the-badge" alt="MackIA" />
</p>

## 🏆 Resultados na Competição

* **Classificação Final:** **46º Lugar** 🏆
* **Evento:** Desafio de Dados do Workshop de Computação Aplicada (**WORCAP**) — Pós-Graduação do Instituto Nacional de Pesquisas Espaciais (**INPE**).
* **Métrica de Avaliação:** *Root Mean Squared Error* (RMSE) na escala de **mm/dia**.

---

## 👥 Integrantes & Desenvolvimento

Este projeto foi desenvolvido por membros da **MackIA** (Liga Acadêmica de Inteligência Artificial da Universidade Presbiteriana Mackenzie):

* 👤 **[Nome do Integrante 1]**
* 👤 **[Nome do Integrante 2]**
* 👤 **[Nome do Integrante 3]**

---

## 💡 Sobre o Problema

A chuva é uma das variáveis meteorológicas mais difíceis de prever no mundo. Na América do Sul, a presença da Amazônia e da Cordilheira dos Andes cria padrões de clima complexos que variam drasticamente de uma região para outra. Saber antecipadamente quanta chuva vai cair no próximo mês é crucial para prevenir enchentes, gerenciar reservatórios de hidrelétricas, proteger colheitas na agricultura e planejar secas.

**O Desafio:** Construir um modelo de Inteligência Artificial capaz de olhar para o mapa meteorológico do mês atual (vento, umidade, pressão, temperatura) e prever com precisão a **média de chuva diária (em mm/dia)** de cada região da América do Sul no **mês seguinte**.

---

## 🔬 Abordagem Técnica (Para Computação & Data Science)

Em vez de tratar o clima como tabelas isoladas, formulamos o problema como uma **sequência de vídeo/imagens spatiotemporais**. Cada mês é representado por uma grade geográfica de coordenadas ($\text{Latitude} \times \text{Longitude}$) contendo 9 canais físicos (camadas).

Usamos uma arquitetura baseada em **ConvLSTM2D**, que combina a capacidade das **Convoluções (CNNs)** de extrair padrões espaciais do mapa com a capacidade de memória temporal das **LSTMs (RNNs)**.

---

## 📂 Arquitetura do Repositório

```text
├── Notebook/
│   └── hackathon-2026-precipita-o.ipynb   # Notebook completo (EDA, Treinamento, ConvLSTM e Submissão)
└── README.md                              # Documentação oficial do projeto

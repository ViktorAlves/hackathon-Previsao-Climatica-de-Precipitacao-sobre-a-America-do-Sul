# 🌧️ Previsão de Precipitação na América do Sul — Hackathon WORCAP 2026 (INPE)

<p align="center">
  <img src="https://img.shields.io/badge/Python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54" alt="Python" />
  <img src="https://img.shields.io/badge/TensorFlow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white" alt="TensorFlow" />
  <img src="https://img.shields.io/badge/Kaggle-20BEFF?style=for-the-badge&logo=Kaggle&logoColor=white" alt="Kaggle" />
  <img src="https://img.shields.io/badge/MackIA-Mackenzie-red?style=for-the-badge" alt="MackIA" />
</p>

## 🏆 Resultados na Competição

* **Classificação Final:** **46º Lugar** 🏆
* **Evento:** Desafio de Dados do Workshop de Computação Aplicada (**WORCAP**) — Pós-Graduação do Instituto Nacional de Pesquisas Espaciais (**INPE**).
* **Métrica de Avaliação:** *Root Mean Squared Error* (RMSE) na escala de **mm/dia**.

---

## 👥 Integrantes & Desenvolvimento

Este projeto foi desenvolvido por membros da **MackIA** (Liga Acadêmica de Inteligência Artificial da Universidade Presbiteriana Mackenzie):

* 👤 **Bruno Antico Galin**
  [![LinkedIn](https://img.shields.io/badge/-LinkedIn_Bruno-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/ana-g-548419230)
  
* 👤 **Lucas Pires de Camargo Sarai**
  [![LinkedIn](https://img.shields.io/badge/-LinkedIn_Lucas-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/ana-g-548419230)
* 👤 **Victor Luiz de Sá Alves**
  [![LinkedIn](https://img.shields.io/badge/-LinkedIn_Victor_Alves-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/victor-luiz-b39738222/)

---

## 💡 Sobre o Problema

A chuva é uma das variáveis meteorológicas mais difíceis de prever no mundo. Na América do Sul, a presença da Amazônia e da Cordilheira dos Andes cria padrões de clima complexos que variam drasticamente de uma região para outra. Saber antecipadamente quanta chuva vai cair no próximo mês é crucial para prevenir enchentes, gerenciar reservatórios de hidrelétricas, proteger colheitas na agricultura e planejar secas.

**O Desafio:** Construir um modelo de Inteligência Artificial capaz de olhar para o mapa meteorológico do mês atual (vento, umidade, pressão, temperatura) e prever com precisão a **média de chuva diária (em mm/dia)** de cada região da América do Sul no **mês seguinte**.

---

## 🔬 Abordagem Técnica e Arquitetural

Para prever a chuva na América do Sul, a maioria dos modelos tradicionais olha apenas para números em tabelas. O problema é que isso ignora algo essencial: **o clima acontece no espaço e muda com o tempo**.

Para resolver isso, nós transformamos o problema do clima em algo parecido com o **processamento de um vídeo**.

---

### 1. Entendendo os "Quadros de Vídeo" Climáticos
Em vez de olhar para dados isolados, cada mês é tratado como uma **imagem do mapa da América do Sul** dividida em uma grade geográfica de alta precisão (vários pontos de Latitude e Longitude).

Uma imagem comum de celular tem 3 camadas de cor (Vermelho, Verde e Azul). Já a nossa "imagem" climática possui **9 camadas de informações físicas** sobrepostas em cada ponto do mapa:

* 🌤️ **Condições de Superfície:** Nuvens no céu, pressão do ar e a quantidade de chuva do mês atual.
* 🌡️ **Temperatura:** Medida em duas alturas diferentes (no solo e na atmosfera).
* 💧 **Umidade do Ar:** A quantidade de vapor de água disponível na atmosfera.
* 💨 **Ventos:** Força e direção do vento (movimentos de norte-sul e leste-oeste).

---

### 2. A Escolha da Inteligência Artificial: ConvLSTM
Modelos comuns de Inteligência Artificial costumam ter limitações ao lidar com o clima:
* **Algoritmos de Imagem (CNNs):** São ótimos para reconhecer mapas e regiões (como identificar a Amazônia ou a Cordilheira dos Andes), mas **não têm memória** do que aconteceu no mês anterior.
* **Algoritmos de Memória (LSTMs):** São ótimos para entender o histórico ao longo do tempo, mas **não sabem ler mapas** nem entender vizinhanças geográficas.

Para unir o melhor dos dois mundos, usamos a **ConvLSTM**: uma tecnologia de rede neural que **combina visão espacial e memória temporal ao mesmo tempo**.

---

### 3. Como o Modelo Toma Decisões
1. **Leitura do Mapa:** A rede recebe o "mapa climático" do mês atual com as 9 camadas de informação.
2. **Análise Espacial e Temporal:** A inteligência aprende como as montanhas, os ventos e a umidade interagem entre as regiões e como esse cenário está mudando de um mês para o outro.
3. **Geração do Resultado:** O modelo sintetiza todo esse conhecimento e desenha um **novo mapa de previsão**, indicando a média de chuva esperada (em mm/dia) para cada ponto da América do Sul no mês seguinte.
---

## 🔒 Acesso aos Dados e Limitações

Por se tratar de um desafio promovido no Kaggle em parceria com o **INPE (WORCAP)**, os arquivos brutos de dados possuem **acesso restrito aos participantes da competição**. 

Devido aos limites de armazenamento do GitHub e aos termos de privacidade/propriedade dos dados, os arquivos brutos **não estão hospedados e nem podem ser distribuídos diretamente neste repositório**.

---

### 🗂️ Estrutura do Dataset Utilizado

Para fins de documentação do projeto e reprodutibilidade do código, o pipeline foi construído para processar os seguintes arquivos no formato NetCDF (`.nc`) e CSV:

#### Preditores Climáticos (Features de Treino)
* ☁️ `treino_cloud_cover.nc` — Cobertura de nuvens[cite: 1]
* 🌐 `treino_geopotential_850.nc` — Geopotencial a 850 hPa[cite: 1]
* 💧 `treino_rel_hum_850.nc` — Umidade relativa a 850 hPa[cite: 1]
* 💧 `treino_shum_850.nc` — Umidade específica a 850 hPa[cite: 1]
* 🎈 `treino_surface_pressure.nc` — Pressão na superfície[cite: 1]
* 🌡️ `treino_t2.nc` — Temperatura a 2 metros do solo[cite: 1]
* 🌡️ `treino_temperature_850.nc` — Temperatura a 850 hPa[cite: 1]
* 🌧️ `treino_tp.nc` — Precipitação acumulada do mês atual[cite: 1]
* 💨 `treino_u_850.nc` — Componente $u$ do vento (leste-oeste) a 850 hPa[cite: 1]
* 💨 `treino_v_850.nc` — Componente $v$ do vento (norte-sul) a 850 hPa[cite: 1]

#### Alvo, Teste e Submissão
* 🎯 `treino_tp_alvo.nc` — Precipitação média do mês seguinte ($\text{mm/dia}$)[cite: 1]
* 🧪 `teste_features.nc` — Dataset de validação/teste da competição[cite: 1]
* 📄 `sample_submission.csv` — Estrutura oficial de submissão do Kaggle[cite: 1]

---

## 📂 Arquitetura do Repositório

```text
├── Notebook/
│   └── hackathon-2026-precipita-o.ipynb   # Notebook completo (EDA, Treinamento, ConvLSTM e Submissão)
└── README.md                              # Documentação oficial do projeto

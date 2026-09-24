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

Em vez de olhar para o clima como uma tabela estática de números (onde a relação de vizinhança entre as cidades se perde), **modelamos o problema como se o clima fosse um "vídeo" em movimento**. 

### 1. A Analogia dos "Quadros de Vídeo" Climáticos
Cada mês da América do Sul é tratado como uma moldura (*frame*) de imagem de alta resolução geográfica (composta por uma grade de coordenadas de **Latitude $\times$ Longitude**). 

No entanto, em vez de uma imagem comum (que possui apenas 3 canais de cor: Vermelho, Verde e Azul), cada pixel da nossa grade possui **9 camadas físicas sobrepostas** no mesmo ponto do espaço:
* 🌤️ **3 Camadas de Superfície:** Cobertura de nuvens, pressão à superfície e chuva do mês atual ($tp$).
* 🌡️ **2 Camadas de Temperatura:** Temperatura a 2 metros do solo e temperatura em altitude (850 hPa).
* 💧 **2 Camadas de Umidade:** Umidade relativa e umidade específica em altitude.
* 💨 **2 Camadas de Vento:** Componentes de vento horizontal ($u$) e vertical ($v$).

---

### 2. Por que usar ConvLSTM2D? (Espaço + Tempo)
Modelos tradicionais de rede neural enfrentam duas limitações no clima:
1. **Redes Convolucionais (CNNs):** Enxergam padrões no espaço (ex: percebem a forma de uma massa de ar frio no mapa), mas **não têm memória** do que aconteceu no mês anterior.
2. **Redes Recorrentes (LSTMs):** Guardam memória temporal (ex: sabem a tendência dos últimos meses), mas **não entendem o mapa** como uma grade geográfica conectada.

A camada **ConvLSTM2D** resolve isso ao unir as duas abordagens no mesmo neurônio: ela aplica filtros de convolução espacial *dentro* dos portões de memória da LSTM. Dessa forma, o modelo consegue aprender simultaneamente:
* **Padrões Espaciais:** Como a Cordilheira dos Andes ou a Bacia Amazônica bloqueiam e direcionam a umidade nas regiões vizinhas.
* **Dinâmica Temporal:** Como esses sistemas de pressão e temperatura evoluem e se deslocam mês a mês.

---

### 3. Mecanismo de Entrada e Saída
* **Entrada no Tensor ($[B, T, \text{Lat}, \text{Lon}, C]$):** O modelo recebe um lote ($B$) contendo a janela de tempo ($T=1$), a dimensão da grade geográfica ($\text{Lat} \times \text{Lon}$) e as $C=9$ variáveis físicas atreladas a cada ponto do mapa.
* **Processamento Convolucional:** A camada `ConvLSTM2D` processa esse mapa mantendo a resolução espacial intacta (`padding="same"`).
* **Projeção Final:** Uma camada `Conv2D(1, kernel_size=1)` comprime as representações ocultas para apenas **1 mapa de saída**, que representa a previsão exata de chuva em $\text{mm/dia}$ para o mês seguinte ($M+1$)..

---

## 📂 Arquitetura do Repositório

```text
├── Notebook/
│   └── hackathon-2026-precipita-o.ipynb   # Notebook completo (EDA, Treinamento, ConvLSTM e Submissão)
└── README.md                              # Documentação oficial do projeto

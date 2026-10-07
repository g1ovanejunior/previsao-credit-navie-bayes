# 📊 Previsão de Risco de Crédito com Gaussian Naive Bayes

Este projeto aplica técnicas de **Ciência de Dados** e **Machine Learning** para prever a concessão de crédito a clientes, classificando-os em bons (`good`) ou maus (`bad`) pagadores. 

A solução compara a performance do modelo preditivo antes e depois de um processo de **Análise Exploratória de Dados (EDA)**, **Engenharia de Atributos** e **Tratamento de Assimetria**.

---

## 🎯 Importância do Projeto e Contexto de Negócio

No setor financeiro, a concessão de crédito envolve o equilíbrio constante entre maximizar os lucros e minimizar a inadimplência. 
- **Conceder crédito a um bom pagador** gera receita por meio dos juros.
- **Conceder crédito a um mau pagador** resulta em prejuízo direto do capital emprestado.

Geralmente, modelos ingênuos ou dados sem tratamento tendem a classificar a maioria dos clientes como "bons pagadores" devido ao desbalanceamento natural da base de dados. O grande diferencial deste projeto foi perceber como o **tratamento estruturado dos dados permitiu identificar uma quantidade significativamente maior de maus pagadores (`bad`)**, reduzindo o risco de concessão indevida e protegendo a instituição financeira de potenciais perdas econômicas.

---

## 📁 Estrutura do Repositório

- `dados/Credit.csv`: Base de dados principal de concessão de crédito.
- `parte1_sem_análise.ipynb`: Pipeline inicial de treinamento do modelo Naive Bayes direto nos dados brutos.
- `parte2_com_analise.ipynb`: Pipeline avançado incluindo análise estatística, seleção de atributos, tratamento de assimetria e reavaliação do modelo.

---

## ⚙️ Etapas do Código e Metodologia

### 1. Modelo Baseline (Sem Análise Detalhada)
No primeiro notebook (`parte1_sem_análise.ipynb`):
- Os dados foram carregados e codificados com `LabelEncoder` para transformar atributos categóricos em numéricos.
- A base foi dividida em **70% para treino** e **30% para teste** (`train_test_split`).
- O algoritmo **Gaussian Naive Bayes** foi treinado diretamente.
- **Resultado:** A acurácia obtida foi de **71%**. No entanto, o modelo apresentou baixo poder de captura de maus pagadores (Recall para a classe `bad` de apenas 48%).

### 2. Pipeline com Análise Exploratória e Engenharia de Dados
No segundo notebook (`parte2_com_analise.ipynb`):
- **Análise de Relevância Categórica (Teste Qui-Quadrado):** Avaliou-se o p-valor das variáveis categóricas em relação à classe alvo. Colunas com p-valor > 0.05 (como `job` e `own_telephone`) foram removidas por não apresentarem relevância estatística.
- **Informação Mútua (Mutual Information):** Identificação e remoção de atributos com score nulo em relação à variável alvo (`installment_commitment`, `residence_since`, `existing_credits`).
- **Tratamento de Assimetria (Yeo-Johnson):** Aplicação do `PowerTransformer` para corrigir a assimetria das variáveis numéricas (`duration`, `credit_amount`, `age`), aproximando-as de uma distribuição normal e adequando-as às premissas do Gaussian Naive Bayes.
- **One-Hot Encoding:** Codificação das variáveis categóricas restantes para evitar ordenação arbitrária.

---

## 📊 Dashboards Interativos (Power BI)

Para complementar a análise técnica e fornecer uma visão executiva para tomada de decisão, foram desenvolvidos dashboards no Power BI.

> ⚠️ **Espaço reservado para visualização dos Dashboards do Power BI:**
> 
> *Insira aqui os links, GIFs ou capturas de tela dos seus painéis.*
> 
> ![Dashboard de Análise de Crédito - Visão Geral](URL_DA_SUA_IMAGEM_1)
> 
> ![Dashboard de Perfil de Risco e Inadimplência](URL_DA_SUA_IMAGEM_2)
> 
> *(Opções adicionais: Você também pode inserir um link direto para o relatório interativo publicado no Power BI Service)*

---

## 📈 Resultados e Comparação de Modelos


Abaixo está a comparação detalhada do modelo **Gaussian Naive Bayes** aplicado antes e depois das etapas de Análise Exploratória de Dados (EDA), tratamento de assimetria (*skewness*) e transformações de atributos.

---

### 1. Matrizes de Confusão

| Modelo | Matriz de Confusão |
| :--- | :--- |
| **Parte 1: Sem Análise Exploratória** | <table><tr><th></th><th>Pred. Bad</th><th>Pred. Good</th></tr><tr><th>True Bad</th><td><b>41</b> (VP)</td><td>45 (FN)</td></tr><tr><th>True Good</th><td>42 (FP)</td><td><b>172</b> (VN)</td></tr></table> |
| **Parte 2: Com Análise Exploratória** | <table><tr><th></th><th>Pred. Bad</th><th>Pred. Good</th></tr><tr><th>True Bad</th><td><b>60</b> (VP)</td><td>30 (FN)</td></tr><tr><th>True Good</th><td>92 (FP)</td><td><b>118</b> (VN)</td></tr></table> |

---

### 2. Tabela Comparativa de Métricas

| Métrica | Sem Análise (Parte 1) | Com Análise (Parte 2) | Variação / Impacto |
| :--- | :---: | :---: | :--- |
| **Acurácia Geral** | **71.00%** | 59.33% | 📉 -11.67% *(Ilusória devido ao desbalanceamento)* |
| **Sensibilidade / Recall (`bad`)** | 47.67% | **66.67%** | 📈 **+19.00%** *(Aumento expressivo na detecção de risco)* |
| **Falsos Negativos (`bad` aprovado)** | 45 | **30** | 📉 **-15 maus pagadores aprovados erroneamente** |
| **Precisão (`bad`)** | 49.40% | 39.47% | 📉 -9.93% |
| **F1-Score (`bad`)** | 0.4852 | **0.4959** | 📈 +0.0107 *(Melhor equilíbrio para a classe minoritária)* |

---

### 3. Análise dos Resultados e Conclusão de Negócio

* **Redução do Risco de Crédito (Falsos Negativos):**
  Sem a análise exploratória, o modelo deixava passar **45 maus pagadores** como se fossem bons clientes (taxa de erro de 52,3% na classe `bad`). Após o tratamento dos dados, o modelo reduziu essa falha para **30 maus pagadores**, aumentando a capacidade de captura de risco (**Recall**) de **47,67% para 66,67%**.

* **Entendendo a Queda na Acurácia Geral:**
  A acurácia geral caiu de 71,00% para 59,33% porque o modelo tornou-se mais conservador, classificando mais clientes como `bad` (gerando mais Falsos Positivos: 92 vs 42). Em problemas de risco de crédito altamente desbalanceados, a acurácia isolada é uma métrica enganosa (*accuracy paradox*), enquanto a **Sensibilidade (Recall)** sobre a classe de risco é a métrica mais crítica para a instituição financeira.

* **Conclusão:**
  A etapa de tratamento de dados e Análise Exploratória ajustou as distribuições para atender melhor às premissas do **Gaussian Naive Bayes**, resultando em um modelo significativamente mais seguro e alinhado aos objetivos reais de mitigação de prejuízos financeiros.
---

## 🛠️ Tecnologias Utilizadas

- **Linguagem:** Python
- **Manipulação de Dados:** Pandas
- **Estatística e Machine Learning:** Scikit-Learn (`GaussianNB`, `PowerTransformer`, `mutual_info_classif`), SciPy (`chi2_contingency`)
- **Visualização de Dados:** Matplotlib, Yellowbrick, Power BI
- **Ambiente:** Jupyter Notebook / VS Code

---

## ✒️ Autor

Desenvolvido por **Giovane Ferreira Silva Junior**.

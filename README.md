# Data-Cleaning-ETL-Project

## Visão Geral do Projeto
No mundo real, os dados chegam desorganizados, com informações em falta e erros de registo que destroem a confiança nos relatórios. Este projeto demonstra a minha capacidade de engenharia de dados ao pegar numa base de dados de vendas "suja" e transformá-la numa tabela 100% limpa, consistente e pronta para análise de negócio.

---

##  O Problema vs. A Solução (Data Quality)

###  1. Antes: Base de Dados Original (Com Erros)
* **Inconsistência Crítica:** Altos índices de valores nulos (`null`) nas colunas de Método de Pagamento (`Payment Method`, com apenas 68% de dados válidos) e Localização (`Location`, com apenas 63% de dados válidos).
* **Dados Corrompidos:** Registos vazios em colunas numéricas estruturais como Quantidade (`Quantity`) e falhas de registo em datas de transações (`Transaction Date`).

<img width="1649" height="847" alt="image" src="https://github.com/user-attachments/assets/377bf839-1789-4842-924c-162a9bf7e6c5" />


---

###  2. Depois: Tabela Tratada e Higienizada
* **Qualidade Máxima:** 100% de dados válidos, 0% de erros e 0% de valores vazios em todas as colunas do relatório.
* **Pronto para Modelação:** Os dados foram normalizados para garantir que qualquer fórmula DAX ou relação de *Star Schema* funcione sem quebrar.

<img width="1651" height="842" alt="image" src="https://github.com/user-attachments/assets/272ea0c5-6439-40da-b69e-743f38523e7e" />


---

##  Transformações Aplicadas (Applied Steps no Power Query)

Para atingir a integridade total dos dados, apliquei as seguintes etapas técnicas no **Power Query**:

1. **Tratamento de Nulos em Dimensões de Texto:** Substituição de valores `null` por **"Desconhecido"** nas colunas de `Payment Method` e `Location`. Isto evita que os filtros do utilizador final mostrem a opção vazia `(blank)`.
2. **Correção de Métricas Numéricas:** Limpeza de anomalias na coluna de `Quantity` e alinhamento com os valores de `Price Per Unit` e `Total Spent`.
3. **Padrão Cronológico:** Higienização e tipagem correta da coluna `Transaction Date` para garantir que o modelo se conecte perfeitamente com uma tabela de calendário (`d_Data`).

---

##  Competências Demonstradas
* Processos de ETL (Extract, Transform, Load) avançados
* Gestão e tratamento de valores nulos (`null`) e em branco
* Análise de qualidade e integridade de dados (Data Profiling)
* Domínio de Microsoft Power Query

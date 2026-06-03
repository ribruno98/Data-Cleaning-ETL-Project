# Data-Cleaning-ETL-Project

## Project Overview (Visão Geral do Projeto)
No mundo real, os dados chegam desorganizados, com informações em falta e erros de registo que destroem a confiança nos relatórios. Este projeto demonstra a minha capacidade de engenharia de dados ao pegar numa base de dados de vendas "suja" e transformá-la numa tabela 100% limpa, consistente e pronta para análise de negócio.

---

##  O Problema vs. A Solução (Data Quality)

###  1. Antes: Base de Dados Original (Com Erros)
* **Inconsistência Crítica:** Altos índices de valores nulos (`null`) nas colunas de Método de Pagamento (`Payment Method`, com apenas 68% de dados válidos) e Localização (`Location`, com apenas 63% de dados válidos).
* **Dados Corrompidos:** Registos vazios em colunas numéricas estruturais como Quantidade (`Quantity`) e falhas de registo em datas de transações (`Transaction Date`).

*(Apaga este texto e faz CTRL+V do teu PRIMEIRO PRINT aqui - o que tem as barras vermelhas/cinzentas)*

---

###  2. Depois: Tabela Tratada e Higienizada
* **Qualidade Máxima:** 100% de dados válidos, 0% de erros e 0% de valores vazios em todas as colunas do relatório.
* **Pronto para Modelação:** Os dados foram normalizados para garantir que qualquer fórmula DAX ou relação de *Star Schema* funcione sem quebrar.

*let
    Origem = Csv.Document(File.Contents("C:\Users\B8bru\Downloads\dirty_cafe_sales.csv"),[Delimiter=",", Columns=8, Encoding=1252, QuoteStyle=QuoteStyle.None]),
    #"Tipo Alterado" = Table.TransformColumnTypes(Origem,{{"Column1", type text}, {"Column2", type text}, {"Column3", type text}, {"Column4", type text}, {"Column5", type text}, {"Column6", type text}, {"Column7", type text}, {"Column8", type text}}),
    #"Cabeçalhos Promovidos" = Table.PromoteHeaders(#"Tipo Alterado", [PromoteAllScalars=true]),
    #"Tipo Alterado1" = Table.TransformColumnTypes(#"Cabeçalhos Promovidos",{{"Transaction ID", type text}, {"Item", type text}, {"Quantity", type text}, {"Price Per Unit", type text}, {"Total Spent", type text}, {"Payment Method", type text}, {"Location", type text}, {"Transaction Date", type text}}),
    #"Duplicados Removidos" = Table.Distinct(#"Tipo Alterado1", {"Transaction ID"}),
    #"Valor Substituído" = Table.ReplaceValue(#"Duplicados Removidos","UNKNOWN",null,Replacer.ReplaceValue,{"Item"}),
    #"Valor Substituído1" = Table.ReplaceValue(#"Valor Substituído","",null,Replacer.ReplaceValue,{"Item"}),
    #"Valor Substituído2" = Table.ReplaceValue(#"Valor Substituído1","ERROR",null,Replacer.ReplaceValue,{"Quantity"}),
    #"Valor Substituído3" = Table.ReplaceValue(#"Valor Substituído2","UNKNOWN",null,Replacer.ReplaceValue,{"Quantity"}),
    #"Valor Substituído4" = Table.ReplaceValue(#"Valor Substituído3","",null,Replacer.ReplaceValue,{"Quantity"}),
    #"Valor Substituído5" = Table.ReplaceValue(#"Valor Substituído4","ERROR",null,Replacer.ReplaceValue,{"Price Per Unit"}),
    #"Valor Substituído6" = Table.ReplaceValue(#"Valor Substituído5","",null,Replacer.ReplaceValue,{"Price Per Unit"}),
    #"Valor Substituído7" = Table.ReplaceValue(#"Valor Substituído6","UNKNOWN",null,Replacer.ReplaceValue,{"Price Per Unit"}),
    #"Valor Substituído8" = Table.ReplaceValue(#"Valor Substituído7","ERROR",null,Replacer.ReplaceValue,{"Total Spent"}),
    #"Valor Substituído9" = Table.ReplaceValue(#"Valor Substituído8","",null,Replacer.ReplaceValue,{"Total Spent"}),
    #"Valor Substituído10" = Table.ReplaceValue(#"Valor Substituído9","UNKNOWN",null,Replacer.ReplaceValue,{"Total Spent"}),
    #"Valor Substituído11" = Table.ReplaceValue(#"Valor Substituído10","UNKNOWN",null,Replacer.ReplaceValue,{"Payment Method"}),
    #"Valor Substituído12" = Table.ReplaceValue(#"Valor Substituído11","",null,Replacer.ReplaceValue,{"Payment Method"}),
    #"Valor Substituído13" = Table.ReplaceValue(#"Valor Substituído12","ERROR",null,Replacer.ReplaceValue,{"Payment Method"}),
    #"Valor Substituído14" = Table.ReplaceValue(#"Valor Substituído13","UNKNOWN",null,Replacer.ReplaceValue,{"Location"}),
    #"Valor Substituído15" = Table.ReplaceValue(#"Valor Substituído14","ERROR",null,Replacer.ReplaceValue,{"Location"}),
    #"Valor Substituído16" = Table.ReplaceValue(#"Valor Substituído15","",null,Replacer.ReplaceValue,{"Location"}),
    #"Valor Substituído17" = Table.ReplaceValue(#"Valor Substituído16","ERROR",null,Replacer.ReplaceValue,{"Transaction Date"}),
    #"Valor Substituído18" = Table.ReplaceValue(#"Valor Substituído17","",null,Replacer.ReplaceValue,{"Transaction Date"}),
    #"Valor Substituído19" = Table.ReplaceValue(#"Valor Substituído18","UNKNOWN",null,Replacer.ReplaceValue,{"Transaction Date"}),
    #"Linhas em Branco Removidas" = Table.SelectRows(#"Valor Substituído19", each not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null}))),
    #"Linhas em Branco Removidas1" = Table.SelectRows(#"Linhas em Branco Removidas", each not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null}))),
    #"Valor Substituído20" = Table.ReplaceValue(#"Linhas em Branco Removidas1",null,"",Replacer.ReplaceValue,{"Transaction Date"}),
    #"Linhas em Branco Removidas2" = Table.SelectRows(#"Valor Substituído20", each not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null}))),
    #"Valor Substituído21" = Table.ReplaceValue(#"Linhas em Branco Removidas2","","876",Replacer.ReplaceValue,{"Transaction Date"}),
    #"Tipo Alterado2" = Table.TransformColumnTypes(#"Valor Substituído21",{{"Transaction Date", type date}}),
    #"Erros Removidos" = Table.RemoveRowsWithErrors(#"Tipo Alterado2", {"Transaction Date"}),
    #"Linhas em Branco Removidas3" = Table.SelectRows(#"Erros Removidos", each not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null}))),
    #"Linhas Filtradas" = Table.SelectRows(#"Linhas em Branco Removidas3", each [Item] <> null and [Item] <> ""),
    #"Linhas Filtradas1" = Table.SelectRows(#"Linhas Filtradas", each [Quantity] <> null and [Quantity] <> ""),
    #"Linhas Filtradas2" = Table.SelectRows(#"Linhas Filtradas1", each [Price Per Unit] <> null and [Price Per Unit] <> ""),
    #"Valor Substituído22" = Table.ReplaceValue(#"Linhas Filtradas2",".",",",Replacer.ReplaceText,{"Price Per Unit"}),
    #"Tipo Alterado3" = Table.TransformColumnTypes(#"Valor Substituído22",{{"Price Per Unit", type number}}),
    #"Linhas Filtradas3" = Table.SelectRows(#"Tipo Alterado3", each [Total Spent] <> null and [Total Spent] <> ""),
    #"Valor Substituído23" = Table.ReplaceValue(#"Linhas Filtradas3",".",",",Replacer.ReplaceText,{"Total Spent"}),
    #"Tipo Alterado4" = Table.TransformColumnTypes(#"Valor Substituído23",{{"Total Spent", type number}}),
    #"Valor Substituído24" = Table.ReplaceValue(#"Tipo Alterado4",null,"Desconhecido",Replacer.ReplaceValue,{"Payment Method"}),
    #"Valor Substituído25" = Table.ReplaceValue(#"Valor Substituído24",null,"Desconhecido",Replacer.ReplaceValue,{"Location"}),
    #"Valor Substituído26" = Table.ReplaceValue(#"Valor Substituído25","Desconhecido","Not Informed",Replacer.ReplaceText,{"Payment Method"}),
    #"Valor Substituído27" = Table.ReplaceValue(#"Valor Substituído26","Desconhecido","Not Informed",Replacer.ReplaceText,{"Location"})
in
    #"Valor Substituído27"*

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

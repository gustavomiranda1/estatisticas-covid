# estatisticas-covid

Passo-a-passo da construção do dashboard com as estatísticas de Covid-19:

1) Extração com MYSQL e Power Query

  Após preenchidas as credenciais de conexão, importei a base DADOS_COVID e modelei através do Power Query:
    
```
let
    Fonte = MySQL.Database("34.95.170.227:3306", "teste_dados", [ReturnSingleDatabase=true]),
    teste_dados_DADOS_COVID = Fonte{[Schema="teste_dados",Item="DADOS_COVID"]}[Data],
    #"Colunas Renomeadas" = Table.RenameColumns(teste_dados_DADOS_COVID,{{"date", "Período"}}),
    #"Linhas Filtradas" = Table.SelectRows(#"Colunas Renomeadas", each ([place_type] = "city")),
    #"Personalização Adicionada" = Table.AddColumn(#"Linhas Filtradas", "TextToBinary", each Text.ToBinary([city], 1252)),
    #"Personalização Adicionada1" = Table.AddColumn(#"Personalização Adicionada", "LinesFromBinary", each Lines.FromBinary([TextToBinary])),
    #"Personalização Adicionada2" = Table.AddColumn(#"Personalização Adicionada1", "Cidade", each Text.Combine([LinesFromBinary])),
    #"Colunas Removidas" = Table.RemoveColumns(#"Personalização Adicionada2",{"LinesFromBinary", "TextToBinary"})
in
    #"Colunas Removidas" 
```

  Nessa modelagem, limpei a coluna de place_type para evitar confundir a informação de estados e cidades.
  Também corrigi o formato ANSI do campo [city] para UTF8, dessa forma exibindo os caracteres especiais.
  
2) Criação de medidas
  
  Criei duas medidas novas: Mortes e Casos, que representam o valor acumulado de ambos até a data da linha em questão.
  
```
Mortes = 
CALCULATE (
    SUM ('teste_dados DADOS_COVID'[new_deaths]),
    FILTER (
        ALL ('teste_dados DADOS_COVID'),
        'teste_dados DADOS_COVID'[Período] <= MAX ( 'teste_dados DADOS_COVID'[Período] )
    )
)
```
  
  ```
  Casos = 
CALCULATE (
    SUM ('teste_dados DADOS_COVID'[new_confirmed]),
    FILTER (
        ALL ('teste_dados DADOS_COVID'),
        'teste_dados DADOS_COVID'[Período] <= MAX ( 'teste_dados DADOS_COVID'[Período] )
    ))
  ```
  
3) Visão final do dashboard
  
  
5) Comentários Finais
  
  Não utilizei o modelo estrela nesse projeto, pois só havia uma fato e nenhuma dimensão relacionada. Devido às análises requisitadas, não vi necessidade de importar   tabelas externas simplesmente para que o modelo fosse dimensional.

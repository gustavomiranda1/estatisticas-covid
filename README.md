# estatisticas-covid

Passo-a-passo da construção do dashboard com as estatísticas de Covid-19:

1) Extração com MYSQL e Power Query
  Após preenchidas as credenciais de conexão, importei a base DADOS_COVID e modelei através do Power Query:
  https://github.com/gustavomiranda1/estatisticas-covid/blob/d15f807f6e36e7df1fc0194c309308bed3d134ab/Casos
  
  Nessa modelagem, limpei a coluna de place_type para evitar confundir a informação de estados e cidades.
  Também corrigi o formato ANSI do campo [city] para UTF8, dessa forma exibindo os caracteres especiais.
  
2) Criação de medidas
  Criei duas medidas novas: Mortes e Casos, que representam o valor acumulado de ambos até a data da linha em questão.
  
  
  
3) Visão final do dashboard
  
  
5) Comentários Finais
  Não utilizei o modelo estrela nesse projeto, pois só havia uma fato e nenhuma dimensão relacionada. Devido às análises requisitadas, não vi necessidade de importar   tabelas externas simplesmente para que o modelo fosse dimensional.

```SQL
#Query para mostrar o tipo de distribuição de tabela(s)
SELECT o.name as tablename, distribution_policy_desc
FROM sys.pdw_table_distribution_properties ptdp
JOIN sys.objects o
ON ptdp.object_id = o.object_id
WHERE ptdp.object_id = object_id('NomeTabela') 
````

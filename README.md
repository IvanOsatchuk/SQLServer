# Querys - Úteis SQL SERVER


```SQL
#Query para mostrar o tipo de distribuição de tabela(s)
SELECT 
	 o.name as tablename, 
	 distribution_policy_desc
FROM 
	 sys.pdw_table_distribution_properties ptdp
JOIN sys.objects o ON ptdp.object_id = o.object_id
WHERE 
	 ptdp.object_id = object_id('datamonster.entregas') --Nome da tabela


#Query para acompanhar as distribuições
SELECT
	  o.name as tableName,
	  pnp.pdw_node_id,
	  pnp.distribution_id,
	  pnp.rows
FROM
	  sys.pdw_nodes_partitions as pnp
	  JOIN sys.pdw_nodes_tables as NTables
	  ON pnp.object_id = NTables.object_id
	  AND pnp.pdw_node_id = NTables.pdw_node_id
	  JOIN sys.pdw_table_mappings as TMap
	  ON NTables.name = TMap.physical_name
	  AND substring(TMap.physical_name,40,10) = pnp.distribution_id
	  JOIN sys.objects as o
	  ON TMap.object_id = o.object_id
WHERE 
	  o.object_id = object_id('datamonster.entregas')  --nome tabela
ORDER BY distribution_id




--QUery para ver o passo a passo da query
SELECT
	  step_index,
	  operation_type
FROM
	  sys.dm_pdw_exec_requests er
	  JOIN sys.dm_pdw_request_steps rs
	  ON er.request_id = rs.request_id
WHERE 
	  er.[label] = 'STATEMENT:DemoQuery';
	  
	  
#Query para analisar execusões de query pelo label
-- A query para ser analisada necessita do comando option (label = 'Label da Query')
SELECT *
FROM sys.dm_pdw_exec_requests
WHERE [label] like 'Label da Query'
````

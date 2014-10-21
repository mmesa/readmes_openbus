# Origen Postfix

1. Métrica *primult_correo* : Primer y ultimo correo por cada mes

- **Origen de datos:** `ob_src_postfix`
- **Tipo:** `Batch`
- **Query Type:** `ID STRING,MES INT,ANO INT,ULTIMO STRING,PRIMERO STRING`
- **Query Select:** `SELECT CONCAT(YEAR(eventTimeStamp),MONTH(eventTimeStamp)) as ID,MONTH(eventTimeStamp) as MES,YEAR(eventTimeStamp) as ANO,MAX(eventTimeStamp) as  ULTIMO,MIN(eventTimeStamp) as PRIMERO`
- **Query From:** `FROM ob_src_postfix`
- **Query Where:** `where MSGID is not NULL and QMGRID is not NULL and USERFROM !='null' GROUP BY MONTH(eventTimeStamp),YEAR(eventTimeStamp)`
- **Timestamp:**`eventtimestamp`
- **Id Es:**
- **Query final:** `CREATE EXTERNAL TABLE primult_correo(ID STRING,MES INT, ANO INT,	ULTIMO STRING,  PRIMERO STRING)	STORED BY 'org.elasticsearch.hadoop.hive.EsStorageHandler' TBLPROPERTIES('es.resource' = 'postfix/primult_correo/','es.mapping.id' = 'ID',	'es.index.auto.create' = 'true','es.id.field' = 'ID');`

  `INSERT INTO TABLE primult_correo SELECT CONCAT(YEAR(eventTimeStamp),MONTH(eventTimeStamp)) as ID,MONTH(eventTimeStamp) as MES,
YEAR(eventTimeStamp) as ANO,MAX(eventTimeStamp) as  ULTIMO,MIN(eventTimeStamp) as PRIMERO FROM postfix_logs where MSGID is not NULL and QMGRID is not NULL and USERFROM !='null' GROUP BY MONTH(eventTimeStamp), YEAR(eventTimeStamp);`


2. Métrica ** : 

- **Origen de datos:** ``
- **Tipo:** ``
- **Query Type:** ``
- **Query Select:** ``
- **Query From:** `FROM ob_src_postfix`
- **Query Where:** ``
- **Timestamp:**`eventtimestamp`
- **Id Es:**

# Origen Bluecoat

####1. Métrica *peticiones_usuario* : Peticiones proxy por usuario

- **Origen de datos:** `ob_src_bluecoat`
- **Tipo:** `Batch`
- **Query Type:** `ID STRING,userCode STRING,eventTimeStamp timestamp,peticiones BIGINT`
- **Query Select:** `SELECT CONCAT(eventTimeStamp,"-",userCode) as ID,userCode,eventTimeStamp,Count(1) as peticiones`
- **Query From:** `FROM ob_src_bluecoat`
- **Query Where:** `WHERE eventTimeStamp>"2014-07-29 10:00:00" and eventTimeStamp<="2014-07-30 10:00:00"
GROUP BY eventTimeStamp,usercode`
- **Timestamp:**`eventtimestamp`
- **Id Es:**
- **Query Hive:** `INSERT INTO TABLE peticiones_usuario SELECT CONCAT(eventTimeStamp,"-",userCode) as ID,userCode,eventTimeStamp,Count(1) as peticiones FROM ob_src_bluecoat GROUP BY eventTimeStamp,usercode;`

***

####2. Métrica *peticiones_dominio_usuario* : Peticiones de dominios por usuario

- **Origen de datos:** `ob_src_bluecoat`
- **Tipo:** `Batch`
- **Query Type:** `ID STRING,usuario_dominio STRING,userCode STRING,requestDomain STRING,eventTimeStamp timestamp,peticiones BIGINT`
- **Query Select:** `SELECT CONCAT(eventTimeStamp,"-",requestDomain,"-",userCode) as ID,CONCAT(requestDomain,"-",usercode) as  usuario_dominio,userCode,requestDomain,eventTimeStamp,Count(1) as peticiones`
- **Query From:** `FROM ob_src_bluecoat`
- **Query Where:** `WHERE eventTimeStamp>"2014-07-26 22:00:00"AND eventTimeStamp <="2014-07-27 10:00:00" GROUP BY eventTimeStamp,usercode,requestDomain`
- **Timestamp:**`eventtimestamp`
- **Id Es:**
- **Query Hive:** `INSERT INTO TABLE peticiones_dominio_usuario SELECT CONCAT(eventTimeStamp,"-",requestDomain,"-",userCode) as ID,CONCAT(requestDomain,"-",usercode) as usuario_dominio,userCode,requestDomain,eventTimeStamp,Count(1) as peticiones FROM ob_src_bluecoat GROUP BY eventTimeStamp,usercode,requestDomain;`

***

####3. Métrica *peticiones_usuario_maquina* : Cantidad de peticiones por usuario y máquina

- **Origen de datos:** `ob_src_bluecoat`
- **Tipo:** `Batch`
- **Query Type:** `ID STRING,usuario_maquina STRING,userCode STRING,clientIP STRING,eventTimeStamp timestamp,peticiones BIGINT`
- **Query Select:** `SELECT CONCAT(eventTimeStamp,"-",clientIP,"-",userCode) as ID,CONCAT(clientIP,"-",usercode) as usuario_maquina,userCode,clientIP,eventTimeStamp,Count(1) as peticiones`
- **Query From:** `FROM ob_src_bluecoat`
- **Query Where:** `WHERE eventtimestamp>"2014-07-25 10:00:00" and eventtimestamp<="2014-07-25 22:00:00" GROUP BY eventTimeStamp,usercode,clientIP`
- **Timestamp:**`eventtimestamp`
- **Id Es:**
- **Query Hive:** `INSERT INTO TABLE peticiones_usuario_maquina SELECT CONCAT(eventTimeStamp,"-",clientIP,"-",userCode) as ID,
CONCAT(clientIP,"-",usercode) as usuario_maquina,userCode,clientIP,eventTimeStamp,Count(1) as peticiones FROM ob_src_bluecoat
GROUP BY eventTimeStamp,usercode,clientIP;`

***

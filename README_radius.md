# Origen Radius

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

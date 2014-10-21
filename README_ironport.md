# Origen Ironport

####1. Métrica *total_mensajes* : Total de mensajes de ironport

- **Origen de datos:** `ob_src_ironport`
- **Tipo:** `Batch`
- **Query Type:** `ID STRING,
eventTimeStamp timestamp,
ICID BIGINT,
MID BIGINT,
INTERFACEIP STRING`
- **Query Select:** `SELECT
CONCAT(REL.ICID,"-",MID) as ID,
eventTimeStamp,
REL.ICID,
MID,
INTERFACEIP
FROM(
select ICID,MID `
- **Query From:** `FROM ob_src_ironport`
- **Query Where:** `WHERE ICID is not NULL AND mid is not NULL AND RID is NULL and mailfrom ="null"
GROUP BY ICID,MID) REL
LEFT JOIN
(
SELECT 
ICID
,eventTimeStamp,INTERFACEIP
from ob_src_ironport 
WHERE ICID is not NULL and INTERFACEIP <>"null"
)ENTR
ON ENTR.ICID=REL.ICID
GROUP BY eventTimeStamp,MID,REL.ICID,INTERFACEIP`
- **Timestamp:**`eventtimestamp`
- **Id Es:**
- **Query Hive:** `INSERT INTO TABLE total_mensajes
SELECT
CONCAT(REL.ICID,"-",MID) as ID,
eventTimeStamp,
REL.ICID,
MID,
INTERFACEIP
FROM(
select ICID,MID 
from ob_src_ironport 
WHERE ICID is not NULL AND mid is not NULL AND RID is NULL and mailfrom ="null"
GROUP BY ICID,MID) REL
LEFT JOIN
(
SELECT 
ICID
,eventTimeStamp,INTERFACEIP
from ob_src_ironport 
WHERE ICID is not NULL and INTERFACEIP <>"null"
)ENTR
ON ENTR.ICID=REL.ICID
GROUP BY eventTimeStamp,MID,REL.ICID,INTERFACEIP;
`

***

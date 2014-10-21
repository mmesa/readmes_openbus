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
GROUP BY eventTimeStamp,MID,REL.ICID,INTERFACEIP;`

***

####2. Métrica *filtro_reputacion* : Correos rechazados por Filtro de Reputación

- **Origen de datos:** `ob_src_ironport`
- **Tipo:** `Batch`
- **Query Type:** `ID STRING,
eventTimeStamp timestamp,
ICID BIGINT,
REPUTATION STRING,
INTERFACEIP STRING`
- **Query Select:** `SELECT
DATOS.ICID as ID,
eventTimeStamp,
DATOS.ICID,
REPUTATION,
INTERFACEIP
FROM 
(
SELECT 
REPUTATION,
eventTimeStamp,
ICID`
- **Query From:** `FROM ob_src_ironport`
- **Query Where:** `WHERE REPUTATION like "%TCPREFUSE%" OR REPUTATION like "%REJECTED%") DATOS
JOIN
(
SELECT 
ICID,
INTERFACEIP
from ob_src_ironport 
WHERE ICID is not NULL and INTERFACEIP <>"null"
) INTERF
ON INTERF.ICID=DATOS.ICID`

- **Timestamp:**`eventtimestamp`
- **Id Es:**
- **Query Hive:** `INSERT INTO TABLE filtro_reputacion
SELECT
DATOS.ICID as ID,
eventTimeStamp,
DATOS.ICID,
REPUTATION,
INTERFACEIP
FROM 
(
SELECT 
REPUTATION,
eventTimeStamp,
ICID
FROM ob_src_ironport
WHERE REPUTATION like "%TCPREFUSE%" OR REPUTATION like "%REJECTED%") DATOS
JOIN
(
SELECT 
ICID,
INTERFACEIP
from ob_src_ironport 
WHERE ICID is not NULL and INTERFACEIP <>"null"
) INTERF
ON INTERF.ICID=DATOS.ICID`

***

####3. Métrica *receptor_no_valido* : Correos descartados por Receptor no válido

- **Origen de datos:** `ob_src_ironport`
- **Tipo:** `Batch`
- **Query Type:** `ID STRING,
eventTimeStamp timestamp,
ICID BIGINT,
MID BIGINT,
DSNBOUNCE STRING,
BOUNCEDESC STRING,
INTERFACEIP STRING`
- **Query Select:** `SELECT
CONCAT(REL.ICID,"-",REL.MID) as ID,
eventTimeStamp,
REL.ICID,
REL.MID,
DSNBOUNCE,
BOUNCEDESC,
INTERFACEIP
FROM
(
SELECT
eventTimeStamp,
MID,
DSNBOUNCE,
BOUNCEDESC`
- **Query From:** `FROM ob_src_ironport`
- **Query Where:** `where BOUNCEDESC like "%nvalid recipient%" AND DSNBOUNCE ="5.1.0") BOUNCE
 LEFT JOIN
(
select ICID,MID 
from ob_src_ironport 
WHERE ICID is not NULL AND mid is not NULL AND RID is NULL
GROUP BY ICID,MID) REL
ON REL.MID=BOUNCE.MID
LEFT JOIN
(
SELECT 
ICID,
INTERFACEIP
from ob_src_ironport 
WHERE ICID is not NULL and INTERFACEIP <>"null"
) INTERF
ON INTERF.ICID=REL.ICID`
- **Timestamp:**`eventtimestamp`
- **Id Es:**
- **Query Hive:** `INSERT INTO TABLE receptor_no_valido
SELECT
CONCAT(REL.ICID,"-",REL.MID) as ID,
eventTimeStamp,
REL.ICID,
REL.MID,
DSNBOUNCE,
BOUNCEDESC,
INTERFACEIP
FROM
(
SELECT
eventTimeStamp,
MID,
DSNBOUNCE,
BOUNCEDESC 
from ob_Src_ironport where BOUNCEDESC like "%nvalid recipient%" AND DSNBOUNCE ="5.1.0") BOUNCE
 LEFT JOIN
(
select ICID,MID 
from ob_src_ironport 
WHERE ICID is not NULL AND mid is not NULL AND RID is NULL
GROUP BY ICID,MID) REL
ON REL.MID=BOUNCE.MID
LEFT JOIN
(
SELECT 
ICID,
INTERFACEIP
from ob_src_ironport 
WHERE ICID is not NULL and INTERFACEIP <>"null"
) INTERF
ON INTERF.ICID=REL.ICID;`

***

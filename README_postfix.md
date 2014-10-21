# Origen Postfix

1. Métrica primult_correo : Primer y ultimo correo por cada mes

- **Origen de datos:**ob_src_postfix
- **Tipo:**Batch
- **Query Type:**ID STRING,MES INT,ANO INT,ULTIMO STRING,PRIMERO STRING
- **Query Select:**SELECT CONCAT(YEAR(eventTimeStamp),MONTH(eventTimeStamp)) as ID,MONTH(eventTimeStamp) as MES,YEAR(eventTimeStamp) as ANO,MAX(eventTimeStamp) as  ULTIMO,MIN(eventTimeStamp) as PRIMERO
- **Query From:**FROM ob_src_postfix
- **Query Where:**where MSGID is not NULL and QMGRID is not NULL and USERFROM !='null' GROUP BY MONTH(eventTimeStamp),YEAR(eventTimeStamp)
- **Timestamp:**eventtimestamp
- **Id Es:**

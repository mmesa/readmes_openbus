# Metadata

Para la generación de la metadata del nuevo origen es necesario ejecutar varias sentencias sql en la base de datos MySql 
instalada en el servidor:

```
        
//insert into origen_estructurado (is_kafka_online, kafka_topic, topology_name, version) values (true, 'ob_src_radius', 'ob_src_radius', 1);
//insert into campos_origen (nombre_campo, tipo_campo, origen_estructurado, orden_en_tabla) values ('EVENTTIMESTAMP', 'timestamp', 1, 1);

        
```

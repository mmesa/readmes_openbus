# Topologías

Es necesario crear un archivo .properties por cada nuevo origen con las siguientes propiedades:

--Configuración acceso HDFS

- **HDFS_URL**=Url y puerto donde está el HDFS
- **HDFS_USER**=Usuario para escribir en HDFS
- **HDFS_OUTPUT_DIR**=Directorio donde se escribirá los datos parseados
- **HDFS_OUTPUT_FILENAME**=Prefijo del nombre del fichero de salida
- **HDFS_OUTPUT_FILE_EXTENSION**=Extensión del fichero de salida
- **INPUT_ORIGIN**=Se usa para especificar de dónde serán leídos los datos, las opciones posible son kafka y disco

--Configuración de KAFKA
- **KAFKA_ZOOKEEPER_LIST**=Lista de url:puerto donde esta corriendo el Zookeeper
- **KAFAKA_BROKER_ID**=Id del broker Kafka
- **KAFKA_TOPIC**=Nombre del tópico Kafka del origen
- **KAFKA_FROM_BEGINNING**=Valor true o false para especificar que Kafka lea desde el principio o no

--Configuración STORM
- **STORM_CLUSTER**=Indica si la topología sera desplegada en el cluster local o servidor, los valores posibles son local o server
- **STORM_BATCH_MILLIS**=Tiempo batch en milisegundos
- **STORM_NUM_WORKERS**=Número de workers
- **STORM_MAX_SPOUT_PENDING**=Número máximo de spouts pendientes
- **STORM_TOPOLOGY_NAME**=Nombre de la topología

--Propiedades de la rotación
- **SIZE_ROTATION_UNIT**=Unidades de medida, los valores posibles son KB/MB/GB/TB
- **SIZE_ROTATION_VALUE**=Valor de la medida
- **TIME_ROTATION_UNIT**=Unidad de tiempo, los valores posibles son SECOND/MINUTE/HOUR/DAY
- **TIME_ROTATION_VALUE**=Valor de tiempo

--Propiedades de la sincronización/escritura a HDFS
- **SYNC_MILLIS_PERIOD**=Periodo en milisegundos en el que los datos parseados serán escritos en disco

--Configuración ElasticSearch
- **ELASTICSEARCH_HOST**=Url donde está instalado Elasticsearch
- **ELASTICSEARCH_PORT**=Puerto donde está instalado Elasticsearch
- **ELASTICSEARCH_NAME**=Nombre de elasticsearch
- **ELASTICSEARCH_CACHE_SEARCH**=Permite cachear las búsquedas en elasticsearch, los valores posibles son true o false


Después será necesario crear 2 clases Java para definir la topología:

- Una clase parseador: Clase que utiliza las funciones de Trident que permite procesar cada registro del tópico Kafka y dividirlo en campos.
- Una clase topology: Clase que submitirá cada topología al cluster de Storm.

Una vez creadas estas clases junto a las que están en el proyecto [TopologyParser] (https://github.com/Produban/openbus/tree/TopologyParsers/topologies) se construirá un archivo .jar

Para ejecutar la topología es necesario desplegar el archivo .jar generado en el entorno de Storm de esta manera:

`./bin/storm jar <JAR FILE> <Main Topology class> <properties file>`

Por ejemplo

`./bin/storm jar topologies-0.0.1-SNAPSHOT.jar com.produban.openbus.topologies.OpenbusPostfixLocationTopology postfixLocation.properties`


[Configuración de la topología de los otros orígenes] (https://github.com/mmesa/readmes_openbus/edit/master/README_topologia_conf.md)

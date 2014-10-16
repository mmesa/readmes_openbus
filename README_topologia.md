# Topologías

Es necesario crear un archivo .properties por cada nuevo origen con las siguientes propiedades:

#Configuración acceso HDFS
HDFS_URL=Url y puerto donde está el HDFS
HDFS_USER=Usuario para escribir en HDFS
HDFS_OUTPUT_DIR=Directorio donde se escribirá los datos parseados
HDFS_OUTPUT_FILENAME=Prefijo del nombre del fichero de salida
HDFS_OUTPUT_FILE_EXTENSION=Extensión del fichero de salida
INPUT_ORIGIN=Se usa para especificar de dónde serán leídos los datos, las opciones posible son kafka y disco

#Configuración de KAFKA
KAFKA_ZOOKEEPER_LIST=Lista de url:puerto donde esta corriendo el Zookeeper
KAFAKA_BROKER_ID=1
KAFKA_TOPIC=op_src_ironport
KAFKA_FROM_BEGINNING=false

#Configuración STORM
STORM_CLUSTER=server
STORM_BATCH_MILLIS=700
STORM_NUM_WORKERS=1
STORM_MAX_SPOUT_PENDING=1
STORM_TOPOLOGY_NAME=ob_src_ironport

#Propiedades de la rotación
SIZE_ROTATION_UNIT=MB
SIZE_ROTATION_VALUE=64
TIME_ROTATION_UNIT=HOUR
TIME_ROTATION_VALUE=1
#Propiedades de la sincronización/escritura a HDFS
SYNC_MILLIS_PERIOD=10000
#Configuración ElasticSearch
ELASTICSEARCH_HOST=180.133.240.42
ELASTICSEARCH_PORT=9300
ELASTICSEARCH_NAME=elasticsearch
ELASTICSEARCH_CACHE_SEARCH=true

- HDFS_URL: HDFS machine and port.
- HDFS_USER: User that will be used to write to HDFS.
- HDFS_OUTPUT_DIR: Output directory where the parsed data will be written.
- HDFS_OUTPUT_FILENAME: Prefix of the name of the output file.
- HDFS_OUTPUT_FILE_EXTENSION: Extension of the output file.
- INPUT_ORIGIN: Used for specifying where the data will be read from. Two values are allowed:
	- kafka: Data will be read from a kafka topic.
	- disco: Data will be read from a local file.
- INPUT_FILE: Input File url. Only used if "INPUT_ORIGIN=disco"
- KAFKA_ZOOKEEPER_LIST: List of Zookeeper for kafka <ip:port>,<ip:config>,...
- KAFAKA_BROKER_ID: Kafkas broker id
- KAFKA_TOPIC: Topic name of which data will be read from.
- KAFKA_FROM_BEGINNING: boolean value (true/false)
- STORM_CLUSTER: If the value is "local" the topology will be deployed in a LocalCluster.
- STORM_BATCH_MILLIS: Batch time width.
- STORM_NUM_WORKERS:Number of workers
- STORM_MAX_SPOUT_PENDING: Number of Max. spout Pending
- STORM_TOPOLOGY_NAME: Topology name

Some parameters for file rotation will be provided.
It is possible to rotate the file's name once it reaches a Max size or a certain amount of time has passed.

- SIZE_ROTATION_UNIT: Size unit. Allowed values KB/MB/GB/TB
- SIZE_ROTATION_VALUE: Size value.
- TIME_ROTATION_UNIT: Time unit. Allowed values SECOND/MINUTE/HOUR/DAY
- TIME_ROTATION_VALUE: Time value
- SYNC_MILLIS_PERIOD: Every "SYNC_MILLIS_PERIOD" millis the parsed data will be written to disk.

In this project 3 different topologies have been created:

- Postfix mailing
- Bluecuat proxy
- IronPort 

For each type of entry 2 classes will be created:

- Origin parser class: Trident BaseFunction which will allow us to process each register from the kafka topic and split it into fields. In this project `ProxyParser`, `PostfixParser` and `IronportParser`.
- Topology submiter: This class will submit each topology to the Storm cluster. In this project `OpenbusProxyTopology`, `OpenbusPostfixTopology` and `OpenbusIronportTopology`.

For running one of the topologies it is only needed to upload the JAR into Storm as follows:

``./bin/storm jar <JAR FILE> <Main Topology class> <properties file>`

For example:

`./bin/storm jar topologies-0.0.1-SNAPSHOT.jar com.produban.openbus.topologies.OpenbusProxyTopology proxy.properties`


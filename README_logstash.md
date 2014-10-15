#Logstash / Kafka

Desde la máquina donde esté instalado Kafka:
- Arrancar el servidor de zookeeper si no lo estuviera ya:
`/root/kafka_2.9.2-0.8.1.1/bin/zookeeper-server-start.sh config/zookeeper.properties &`

- Arrancar el servidor de kafka si no lo estuviera ya:
`/root/kafka_2.9.2-0.8.1.1/bin/kafka-server-start.sh config/server.properties &`

- Crear un topico kafka para el nuevo orígen:
`/root/kafka_2.9.2-0.8.1.1bin/kafka-topics.sh --create --zookeeper 180.133.240.174:218,180.133.240.175:2181,180.133.240.176:2181 --replication-factor 1 --partitions 1 --topic nuevo_topico_origen`


Desde la máquina donde esté instalado Logstash:
- Configurar Logstash para recibir la información del nuevo origen al tópico de kafka creado anteriormente:
	- Editar el archivo /root/software/logstash-1.4.1/conf/kafka.conf. 
	- Añadir en la etiqueta output la condición de filtrado del nuevo origen:
```
else if [program] == "cabecera_propia_del_nuevo_origen"{
	kafka{
		topic_id => "nombre_del_nuevo_topico"
		broker_list => "180.133.240.165:9092,180.133.240.166:9092,180.133.240.167:9092"
		codec => plain {
			format => "%{message}"
		}			
	}
}
```

Desde la máquina donde esté instalado Logstash:
- Parar primero y arrancar después parar el servidor de Logstash con la configuración creada anteriormente:
`nohup /root/software/logstash-1.4.1/bin/logstash -f kafka.conf --log ./logs_logstash.log -w 1 -v &`

- [Configuración previa de Logstash](https://github.com/mmesa/readmes_openbus/blob/master/README_logstash_conf.md)


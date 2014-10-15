#Openbus

Para crear nuevos orígenes es necesario realizar una serie de pasos previos:

- Configurar Kafka y Logstash para la creación del nuevo tópico y origen.
- Generar la metadata con el nuevo origen en la bbdd MySql.
- Generar la tabla Hive inicial con el nuevo origen.
- Crear y desplegar la nueva topología para el nuevo origen.

#Referencias

- [Logstash para Openbus] (#)
- [Scripts de metadata generados] (#)
- [Tablas Hive iniciales] (#)
- [Parseador de topologías] (#)

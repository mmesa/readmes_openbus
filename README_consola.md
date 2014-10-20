# Consola Web

El proyecto web_console está subido al github en este [enlace](https://github.com/Produban/openbus/tree/web_console).  
Para poder compilarlo y desplegarlo hay que descargarlo y utilizar un IDE como Eclipse.  
Cuando esté descargado se podrá importar como proyecto maven desde eclipse en la ruta \openbus-web_console\web_console.  
Con la opción maven install se generará el archivo web_console.war que podrá ser desplegado en el servidor de aplicaciones Tomcat
que está instalado en 192.168.x.x/root/software/apache_tomcat_xxx.   
En el directorio /root/software/apache_tomcat_xxx/webapps se copiará el archivo web_console.war generado, antes de eso habrá que
realizar una serie de pasos previos para poder instalar el archivo de forma satisfactoria desde el servidor 192.168.x.x:
* Buscar la referencia al servidor tomcat ´ps -ef |grep tomcat´
* Parar el servidor tomcat con el pid referenciado anteriormente `kill -9 [PID]`
* Borrar cualquier referencia a la aplicación web antigua con `rm -RF /root/software/apache_tomcat_xxx/webapps/web_console*`
* Copiar el nuevo archivo .war generado al directorio destino `cp [dir_origen] /root/software/apache_tomcat_xxx/webapps`
* Arrancar el servidor Tomcat `./root/software/apache_tomcat_xxx/bin/startup.sh`



Para crear nuevas métricas desde consola web se accede a través de : http://192.168.x.x:8080/web_console/  
Usuario: admin     
Password: admin    
Se podrán crear métricas batch u online. Para las batch

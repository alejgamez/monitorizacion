
##ACCESO A LA MAQUINA DE VAGRANT  
  Ejecutamos :
  
```
   cd coreos-vagrant/
   vagrant up #Esto nos levantará la maquina de Vagrant 
   vagrant ssh #Esto nos permitira el acceso a la Maquina con CoreOS vía ssh
```


##PREPARANDO LAS MÁQUINAS CON DOCKER   
  Una vez estamos en la máquina levantamos los containers de docker con el entorno preparado  :
  
```
   docker start kafka1
   docker start kafka2
   docker start druid
```

De esta manera tendremos levantadas las máquinas ya configuradas con las diferentes herramientas . En primer lugar la máquina kafka1 será la que tendrá configurado el zookeeper y el master de Kafka .

La máquina Kafka2 incluye un nodo esclavo de Kafka . 

Y la máquina Druid incluye un mysql y Druid . 

##ACTIVANDO KAFKA1   
  Accedemos a la maquina por ssh:
  
```
   ssh root@172.18.0.2
   password: screencast
```

En primer lugar levantaremos zookeeper(es necesario levantarlo tanto para ejecutar Kafka como para Druid )  para ello ejecutamos los siguientes comandos:

```
   cd kafka/zookeeper-3.4.6
   ./bin/zkServer.sh start
```
A continuación vamos a ejecutar el servidor de Kafka : 

```
   cd ../kafka_2.10-0.8.2.1/
   bin/kafka-server-start.sh config/server.properties &
```

##ACTIVANDO KAFKA2

Accedemos a la maquina por ssh:
  
```
   ssh root@172.18.0.3
   password: screencast
```

A continuación vamos a ejecutar el servidor de Kafka que ya lo tenemos apuntando al maestro para que se unan y trabajen en cluster : 

```
   cd ../kafka_2.10-0.8.2.1/
   bin/kafka-server-start.sh config/server-1.properties &
```

##ACTIVANDO DRUID

Accedemos a la maquina por ssh:
  
```
   ssh root@172.18.0.4
   password: screencast
```

A continuación vamos a ejecutar el servidor de DRUID al que tendremos que haberle indicado previamente donde se encuentra nuestro zookeeper entre otras configuraciones que comento en la configuración de cada herramienta. Por otro lado para habilitar druid en modo realtime hay que levantar 4 componenter de druid y mysql para la gestion de los metadatos: 

```
   cd druid/druid-0.7.3/
   /etc/init.d/mysql start
```
```
# Broker
$ java -Xmx512m -Duser.timezone=UTC -Dfile.encoding=UTF-8 \
     -classpath "config/_common:config/broker:lib/*" \
     io.druid.cli.Main server broker
```

```
# Coordinator
$ java -Xmx512m -Duser.timezone=UTC -Dfile.encoding=UTF-8 \
     -classpath "config/_common:config/coordinator:lib/*" \
     io.druid.cli.Main server coordinator
```

```
# Historical node
$ java -Xmx512m -Duser.timezone=UTC -Dfile.encoding=UTF-8 \
     -classpath "config/_common:config/historical:lib/*" \
     io.druid.cli.Main server historical
```

```
# Realtime node
$ java -Xmx512m -Duser.timezone=UTC -Dfile.encoding=UTF-8 \
     -Ddruid.realtime.specFile=../realtime/druid_config.json \
     -classpath "config/_common:config/realtime:lib/*" \
     io.druid.cli.Main server realtime
```
##ACTIVANDO GRAFANA

En la misma máquina que Druid activamos grafana ejecutando el servidor nginx que hemos configurado:
  
```
   /etc/init.d/nginx start
```


Para poder visualizar los datos que estamos generando accedemos desde la maquina  anfitriona en el navegador por la url : 

http://localhost:3001/

Y para generar datos a kafka incluyo un programa jmeter que genera mensajes escribiendo a localhost:9092 .

##JMETER

Accedemos a la ruta y lo ejecutamos:
  
```
  
   cd ../project/jmeter/apache-jmeter-2.13/
   bin/jmeter
   
```

Una vez dentro de jmeter el jmx es el kafka.jmx

##CONFIGURACION KAFKA
  Para instalarlo ejecutamos :
  
```
wget http://mirrors.maychuviet.vn/apache/kafka/0.8.2.1/kafka_2.11-0.8.2.1.tgz
tar -xvzf kafka_2.11-0.8.2.1.tgz
cd kafka_2.11-0.8.2.1/

```


Para ejecutarlo en la máquina kafka1 : 
  
```
   bin/kafka-server-start.sh config/server.properties
```

Para ejecutarlo en la máquina kafka2 : 
  
```
   bin/kafka-server-start.sh config/server-1.properties
```

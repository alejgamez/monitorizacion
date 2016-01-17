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

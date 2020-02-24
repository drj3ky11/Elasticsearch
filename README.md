# Curso Elasticsearch engineer

Básicos de Elasticsearch, lucene, cluster... (ampliar) Cada shard es un Lucene, gestionan los indices.

El docker-compose de [tres nodos](https://github.com/drj3ky11/Elasticsearch-course/blob/master/elk-3cluster.yml) inicial levanta tres elasticsearch, un cerebro para la gestión de nodos y kibana. Para ello hay que dar permisos chmod -R 777 a cada carpeta en la que están los volúmenes. También atento a editar:

`systctl vm.max_map_count=262144`

Que también se puede editar el sysctl.conf en /etc añadiendo vm.max_map_count=262144

## 1.Nodos

**Ingesta**, mediante maping. A continuación la **Indexación** Para tener buenos resultados los índices tienen que estar en la RAM,durante la ingesta de lo que más tiraré será de la CPU. **Coordinación de la búsqueda** y por último **gestión del cluster.**  Estos serán los cuatro tipos de nodos que tendremos:

+ Máster
+ Ingesta
+ Indexación
+ Cliente

Sólo quiero un nodo como maestro, cuando levanto con docker lo que le paso son *variables de entorno*, no afectan al elasticsearch.yml, no lo varían pero prevalecen. En un cluster de producción tendré como mucho dos máster, uno funcionando y otro de backup. Dedicados únicamente a ser **máster.** Puede dar problemas al perder la conexión entre dos maestros, porque van a estar los dos levantados y estaré generando dos cluster independientes... esto es un problema. Por eso preferimos tres nodos, de forma que el máster se elija por mayoría; tres posibles maestros, solo dos elegibles pero los tres eligen, con esto evito que se levanten dos en simultaneo.

La ingesta también la sacaremos independiente como nodo exclusivo.

Las app irán contra un balanceador, este hablará con los nodos clientes, que a su vez hablarán con los de datos e ingesta. A parte estarán los maestro que hablarán con todos. Lo más importante es aislar los nodos de ingesta y de datos de la red publica. Interesa tres redes, hacia fuera, la interna y otra de gestión de cuota.Hablan por http, lo mejora https por lo evitar la suplantación. Para esto necesitamos certificados.

## Gestión de memoria

Los **nodos Data** son los que más RAM van a requerir. Garvage Colector, repasa la RAM, marca que puede borrar y en una segunda etapa los borra. No poner más de 32GB de lo que en Java se llama *heap.* Java es medio interpretado y medio no, se compila contra la máquina virtual de Java... Hay que controlar la memoria que se está consumiendo dentro de la mv de java, yo lo único que le puedo configurar es cuanta memoria le dejo a su disposición en el heap. 
El **maestro** con 1GB le vale si no es muy exigente, entre 1 y 4 se moverá siempre. En cuanto a los nodos de ingesta lo mismo, lo que más consumirán estos será CPU. Los nodos **clientes** necesitan tanto RAM como CPU

## 2. Práctica 1

Levantamos el cluster [de 7 nodos](https://github.com/drj3ky11/Elasticsearch-course/blob/master/elk-cluster7node.yml) para ello hemos editado el docker compose de 3 nodos que teníamos anteriormente.

Una vez levantados, en cerebro vemos con una estrella quíen es el máster, podemos tirarlo con `docker-compose stop es0*`y vemos quíen pasa a sustituirlo como máster. Para levantarlo de nuevo `docker-compose up -d es0*`


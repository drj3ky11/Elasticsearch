# Elasticsearch


[docker-compose de tres nodos](https://github.com/drj3ky11/Elasticsearch-course/blob/master/elk-3cluster.yml) inicial levanta tres elasticsearch, un cerebro para la gestión de nodos y kibana. Para ello hay que dar permisos chmod -R 777 a cada carpeta en la que están los volúmenes. También atento a editar:

`systctl vm.max_map_count=262144`

Que también se puede editar el sysctl.conf en /etc añadiendo vm.max_map_count=262144

[Cluster de 7 nodos](https://github.com/drj3ky11/Elasticsearch-course/blob/master/elk-cluster7node.yml)
Para levantar antes es recomendable ir a /var/elasticsearch y borrar y volver a crear las carpetas data0* cada vez que levantemos en el mismo sistema. Lo mismo, hacemos `docker ps -a` para ver que contenedores tenemos y borrar todos `docker rm nombre`

Una vez levantados, en cerebro vemos con una estrella quíen es el máster, podemos tirarlo con `docker-compose stop es0*`y vemos quíen pasa a sustituirlo como máster. Para levantarlo de nuevo `docker-compose up -d es0*`


[En este docker-compose](https://github.com/drj3ky11/Elasticsearch-course/blob/master/nodes-clusterespecific.yml) seleccionamos el **1 y 2 como master** elegibles, el tres también lo es pero lo que pretendo es que vote. Como **data** el 3 y 4, el 5 como **ingest** y el 6 y 7 como **cliente**

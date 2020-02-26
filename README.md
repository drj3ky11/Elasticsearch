# Elasticsearch

Vease la [Wiki](https://github.com/drj3ky11/Elasticsearch/wiki)
Recomendable más de 10Gb de ram para el de 7 nodos. Desplegar en servidor virtualizado y docker. Recomendable también cambiar las contraseñas y usuarios que están por defecto.

## FASE FINAL: La carpeta elk
En ella está toda la estructura necesaria para que levante los nodos y securizados.

## Paso a paso
El primer [docker-compose de tres nodos](https://github.com/drj3ky11/Elasticsearch-course/blob/master/elk-3cluster.yml) inicial levanta tres elasticsearch, un cerebro para la gestión de nodos y kibana. Para ello hay que dar permisos chmod -R 777 a cada carpeta en la que están los volúmenes. También atento a editar:

`sysctl vm.max_map_count=262144`

Que también se puede editar el sysctl.conf en /etc añadiendo vm.max_map_count=262144

# Curso Elasticsearch engineer

Básicos de Elasticsearch, lucene, cluster... (ampliar) Cada shard es un Lucene, gestionan los indices.

## 1.Nodos

**Ingesta**, mediante maping. A continuación la **Indexación** Para tener buenos resultados los índices tienen que estar en la RAM,durante la ingesta de lo que más tiraré será de la CPU. **Coordinación de la búsqueda** y por último **gestión del cluster.**  Estos serán los cuatro tipos de nodos que tendremos:

+ Máster
+ Ingesta
+ Indexación
+ Cliente

Sólo quiero un nodo como maestro, cuando levanto con docker lo que le paso son *variables de entorno*, no afectan al elasticsearch.yml, no lo varían pero prevalecen. En un cluster de producción tendré como mucho dos máster, uno funcionando y otro de backup. Dedicados únicamente a ser **máster.** Puede dar problemas al perder la conexión entre dos maestros, porque van a estar los dos levantados y estaré generando dos cluster independientes... esto es un problema. Por eso preferimos tres nodos, de forma que el máster se elija por mayoría; tres posibles maestros, solo dos elegibles pero los tres eligen, con esto evito que se levanten dos en simultaneo.

La ingesta también la sacaremos independiente como nodo exclusivo.

Las app irán contra un balanceador, este hablará con los nodos clientes, que a su vez hablarán con los de datos e ingesta. A parte estarán los maestro que hablarán con todos. Lo más importante es aislar los nodos de ingesta y de datos de la red publica. Interesa tres redes, hacia fuera, la interna y otra de gestión de cuota.

Hablan por http, lo mejora https por lo evitar la suplantación. Para esto necesitamos certificados.

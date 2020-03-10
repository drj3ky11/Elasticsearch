# Elasticsearch
In this repo I will try to make a one docker-compose creata a **elasticsearch stack**, in the folder elk there is without logstash; the second folder, logstash, has a logstash's docker-compose.

Some theory is explained in the  [Wiki](https://github.com/drj3ky11/Elasticsearch/wiki)

Also a good read is [this blog](https://www.elastic.co/es/blog/a-full-stack-in-one-command)

## Requisites
More than **10Gb of RAM**
When running in Ubuntu it would be necessary to set:

`sysctl vm.max_map_count=262144`

Also you can edit the sysctl.conf en /etc adding  vm.max_map_count=262144

Change passwords and users for your own safety.



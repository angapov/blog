---
layout: post
title:  "Quick installation of EFK stack on Kubernetes"
date:   2018-10-24 17:04:00 +0530
tags: [kubernetes, efk, elk, logging]
categories: [kubernetes, logging]
---
This solution describes how to very quickly install Elastisearch, Fluentd and Kibana on Kubernetes. It is taken from [habr.com][source] and requires working Helm in cluster.

```
helm install stable/elasticsearch --namespace logging --name es --set master.persistence.size=10Gi
```
This by default will create 3 master pods, 2 data pods and 2 client pods of Elasticsearch. Wait for pods to be all 1/1 Running.
```
helm install stable/fluentd-elasticsearch --namespace logging --name fluentd --set elasticsearch.host=es-elasticsearch-client
```
This creates DaemonSet on all nodes for fluentd agent.
```
helm install --namespace logging --set env.ELASTICSEARCH_URL=http://es-elasticsearch-client:9200,env.LOGGING_QUIET=true --name kibana stable/kibana
```
This creates pods with Kibana. The only thing left if to create proper ingress and head to dashboard.

Next step is to configure Kibana. Go to dashboard and press "Index Patterns":

![elk1](/images/elk1.png)
![elk2](/images/elk2.png)

Choose @timestamp.

![elk3](/images/elk3.png)

And here it works!

![elk4](/images/elk4.png)

[source]: https://habr.com/post/426959

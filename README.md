# Tyk OSS Deployment

1. Deploy Tyk APIG, Redis, ElasticSearch / Kibana, Tyk Pump & Tyk Operator
```
$ ./launch.sh all
```

2.  After all resources are running, create APIs
```
$ kubectl apply -f ./apis/.
```

3. Can port-forward Kibana and Gateways to access both:

```
$ kubectl port-forward svc/tyk-svc 8080:8080
...
$ kubectl port-forward deployment/kibana 5601
```

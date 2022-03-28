# Declarative API testing using Postman's Newman SDK

1. Deploy our stack (Tyk APIG, Redis, ElasticSearch / Kibana, Tyk Pump & Tyk Operator)
```
$ ./launch.sh all
```

2.  After our stack is running, create API
```
$ kubectl create ns apps
namespace/apps created

$ kubectl apply -f ./apis/httpbin.yaml
deployment.apps/httpbin created
service/httpbin created
apidefinition.tyk.tyk.io/httpbin created
```

3. Can port-forward Kibana and Gateways to access both:

```
$ kubectl port-forward svc/tyk-svc 8080:8080
...
$ kubectl port-forward deployment/kibana 5601
```

4. Test with curl:
```
$ curl localhost:8080/httpbin/get

{
  "args": {},
  "headers": {
    "Accept": "*/*",
    "Accept-Encoding": "gzip",
    "Host": "httpbin.apps:8000",
    "User-Agent": "curl/7.77.0"
  },
  "origin": "127.0.0.1",
  "url": "http://httpbin.apps:8000/get"
}
```

5. Install the newman SDK first, then run the test suite

```
$ npm install -g newman   
```

Run suite:
```
$ newman run postman_example.postman_collection.json 


newman

Postman <> Tyk <> Newman example

→ Httpbin GET
  GET http://localhost:8080/httpbin/get [200 OK, 621B, 211ms]
  ✓  Status code is 200
  ✓  Body to be JSON
  ✓  Response body to be correct

┌─────────────────────────┬────────────────────┬───────────────────┐
│                         │           executed │            failed │
├─────────────────────────┼────────────────────┼───────────────────┤
│              iterations │                  1 │                 0 │
├─────────────────────────┼────────────────────┼───────────────────┤
│                requests │                  1 │                 0 │
├─────────────────────────┼────────────────────┼───────────────────┤
│            test-scripts │                  1 │                 0 │
├─────────────────────────┼────────────────────┼───────────────────┤
│      prerequest-scripts │                  0 │                 0 │
├─────────────────────────┼────────────────────┼───────────────────┤
│              assertions │                  3 │                 0 │
├─────────────────────────┴────────────────────┴───────────────────┤
│ total run duration: 241ms                                        │
├──────────────────────────────────────────────────────────────────┤
│ total data received: 345B (approx)                               │
├──────────────────────────────────────────────────────────────────┤
│ average response time: 211ms [min: 211ms, max: 211ms, s.d.: 0µs] │
└──────────────────────────────────────────────────────────────────┘
```

Read more about Postman's [newman][0] SDK for writing automatable unit tests.

[0]: https://learning.postman.com/docs/writing-scripts/test-scripts/

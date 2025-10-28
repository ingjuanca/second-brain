
---

```Splunk
index=wdpr-ecommerce ecs_task_definition="*avail*"
| fields Msg
| rex field=_raw "Correlation-Id\":\"(?<correlation_id>.*)\",\"X-Conversation-Id" 
| rex field=_raw "duration\\\\\":(?<response_millis>.*),\\\\\"localEndpoint"
| dedup correlation_id, response_millis
| stats count, avg(response_millis), p95(response_millis), p99(response_millis)
```


|                                                                                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| index=wdpr-ecommerce ecs_cluster="dlr-ecommerce-S0001477-usw2-*" ecs_task_definition="evas-svc*"<br><br>  <br>index=wdpr-ecommerce ecs_cluster="dlr-ecommerce-S0001477-usw2-*" ecs_task_definition="evasint*" |

wdpr-ecommerce
wdw_evas
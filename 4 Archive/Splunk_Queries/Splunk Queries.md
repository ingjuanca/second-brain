
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



```Splunk 
index=wdpr-ecommerce ecs_cluster="dlr-ecommerce-S0001477-usw2-lod" ecs_task_definition="cme-elig-load*"
"*"
"Boundary Time" eligibility/api/v2/guests/*/reservations 
| fields Msg
| rex field=_raw "Correlation-Id\":\"(?<correlation_id>.*)\"," 
| rex field=_raw "Boundary Time:\s(?<response_millis>\d+\.\d+)"
| dedup correlation_id, response_millis
| stats count as numberOfReservation, p95(response_millis) as P95RPT, p99(response_millis) as P99RPT, avg(response_millis) as AVGRPT
```

**Explicación del patrón:**

- `Boundary Time:` → busca la cadena literal.
- `\s` → espacio en blanco.
- `(?<response_millis>\d+\.\d+)` → captura en el grupo `response_millis`:
    - `\d+` → uno o más dígitos.
    - `\.` → el punto decimal.
    - `\d+` → uno o más dígitos después del punto.

---


```Splunk
index=WDPR-ecommerce* ecs_cluster="dlr-ecommerce-S0001477-USW2-LOD" ecs_task_definition="CME-AVAIL-load*" "availability/api/v2/availabilities" "sku" brave.Tracer NOT product-types
| fields Msg
| rex field=_raw "Correlation-Id\":\"(?<correlation_id>.*)\",\"X-Conversation-Id" 
| rex field=_raw "duration\\\\\":(?<response_millis>.*),\\\\\"localEndpoint"
| dedup correlation_id, response_millis
| stats count as numberOfReservation, p95(response_millis) as P95RPT, p99(response_millis) as P99RPT, avg(response_millis) as AVGRPT
| eval P95RPT = round(P95RPT, 2)
| eval P99RPT = round(P99RPT, 2)
| eval AVGRPT = round(AVGRPT, 2)
| eval numberOfReservation = tostring(numberOfReservation, "commas")
| eval P95RPT = tostring(P95RPT, "commas")
| eval P99RPT = tostring(P99RPT, "commas")
| eval AVGRPT = tostring(AVGRPT, "commas")
```


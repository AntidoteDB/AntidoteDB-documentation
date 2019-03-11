# Monitoring Antidote with Prometheus

Antidote is configured to expose metrics to [Prometheus](https://prometheus.io/).
At present, the metrics collected are operations per second, open transactions, staleness, aborted transactions and errors together with several virtual machine metrics.

Follow the steps to start one Antidote node and view the metrics.

```bash
cd monitoring
docker-compose up -d
```
This starts one Antidote node, a prometheus server and a graphana database.
If the containers started with no problems, open
`http://localhost:9090/targets` in a web-browser. You will see targets antidote and prometheus.

Now go to
`http://localhost:3000/login` and login with
`admin:admin`


Click `Add data source`.

Enter following details:  
Name: *SomeDataSourceName*  
Type: Prometheus

Url : http://prometheus:9090   
Access: proxy

Click **Add**.

Go To
Dashboard -> import new dashboard.  
Import file `$ANTIDOTE/monitoring/Antidote-Dashboard.json`

To view the monitoring data of antidote, go to Dashboard and open Antidote from the list of dashboards.

To shutdown containers:
`docker-compose down`
# Monitoring Antidote with Prometheus

Antidote is configured to expose metrics to [Prometheus](https://prometheus.io/) via the [antidote_stats module](https://github.com/AntidoteDB/antidote_stats).

Follow the steps to start one Antidote node and view the metrics.

```bash
cd monitoring
docker-compose up -d
```
This starts one Antidote node, a prometheus server and a graphana database.
If the containers started with no problems, open
`http://localhost:9090/targets` in a web-browser. You will see targets antidote and prometheus.

To view the monitoring data of antidote, go to Dashboard and open Antidote from the list of dashboards.

To shutdown containers:
`docker-compose down`



# Adding Metrics to Antidote

To add a metric collection call to the Antidote source code, use the macro `?STATS(term)`, 
where `term` is the term to send to the stat collector process.
The actual Prometheus processing is done in the `antidote_stats` module and every term should be implemented there.
Unknown terms which are not yet supported are logged during runtime, but will not crash the system.


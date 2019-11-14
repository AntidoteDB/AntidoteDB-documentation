# Monitoring Antidote with Prometheus

Antidote is configured to expose metrics to [Prometheus](https://prometheus.io/) via the [`antidote_stats` module](https://github.com/AntidoteDB/antidote_stats).
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




# Adding Metrics to Antidote

To add a metric collection call to the Antidote source code, use the macro `?STATS(term)`, where `term` is the term to send to the stat collector process.
The actual Prometheus processing is done in the `antidote_stats` module and every term should be implemented there.
Unknown terms which are not yet supported are logged as warnings during runtime.

When implementing new metrics in the `antidote_stats` repository, it is useful to work on and implement features locally.
In the main `antidote` folder, create a folder named `_checkouts`. 
Create a symbolic link to the checked out `antidote_stats` repository in that folder.
This overrides the `antidote_stats` dependency with the local one, allowing the treatment of the two different projects as a single unified project.
See the [checkouts](https://www.rebar3.org/docs/dependencies#section-checkout-dependencies) section of `rebar3`.
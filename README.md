# Trino and monitoring

This is a personal exercise for myself to understand Trino and monitoring, including metrics and logs to enhance its observability. The metrics are exported from Trino with JMX-exporter run alongside and pull by Prometheus, and further visualized with Grafana; while the logs are collected by Fluentd with EFK stack (Elasticsearch-Fluentd-Kibana). 

## Architechure Diagram

## Run Locally with Docker-compose
Step 0 - Go to the docker-compose directory
```bash
cd docker-compose
```
Step 1 - Implement with docker-compose
```bash
docker-compose up -d
```
Step 2 - Play around with Trino
- open trino in terminal
```
trino --server localhost:8080 --catalog tpcds --schema sf10
```
- Make some queries like:
```sql
SHOW tables;
SELECT * FROM catalog_sales LIMIT 100;
```
Step 3 - Check metrics with Prometheus
- Before checking with Prometheus web UI, you can browse to localhost:9483 to see if JMX exporter operates correctly.
- Browse to localhost:9090 to checking with Prometheus web UI. 
Step 4 - Check metrics with Grafana
- Browse to localhost:3001 with default account and password(admin/admin)
- Add data source for Prometheus
- Import Dashboard with ID or JSON.(No results while search trino on grafan dashboard and get ID: 10866 while search presto but it is kind ot outdated)
Step 5 - Confirm Logs from Kibana
- Browse to http://localhost:5601/app/management/kibana/indexPatterns and set up the index name pattern for Kibana. Specify fluentd-* to Index name or pattern and click Create.
- Then, go to Discover tab to check the logs. As you can see, logs are properly collected into the Elasticsearch + Kibana, via Fluentd.

Ref:
- https://trino.io/docs/current/admin/jmx.html
- https://github.com/prometheus/jmx_exporter
- https://docs.fluentd.org/container-deployment/docker-compose
- https://stackoverflow.com/questions/71120621/efk-system-is-build-on-docker-but-fluentd-cant-start-up
- https://www.josehisse.dev/blog/como-exportar-metricas-do-trino-para-o-prometheus

Some refs may need to be checked when implement with k8s:
- https://artifacthub.io/packages/helm/presto-loadbalancer/trino
- https://github.com/bluecatengineering/bunny-metrics
  
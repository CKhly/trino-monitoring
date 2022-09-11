# Trino and monitoring

This is a personal exercise for myself to understand Trino and monitoring, including metrics and logs to enhance its observability. The metrics are exported from Trino with JMX-exporter run alongside and pull by Prometheus, and further visualized with Grafana; while the logs are collected by Fluentd with EFK stack (Elasticsearch-Fluentd-Kibana). 

## Architechure Diagram
<img width="802" alt="Screen Shot 2022-09-12 at 12 53 52 AM" src="https://user-images.githubusercontent.com/84711996/189540138-fdec6ee5-eb1f-4f46-83d4-b1a1ea8b2d4f.png">

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
- Browse to localhost:9090 to checking with Prometheus web UI. In the Prometheus interface, place query like:
```
trino_execution_QueryManager_CompletedQueries_TotalCount
```
<img width="1440" alt="Screen Shot 2022-09-12 at 1 14 50 AM" src="https://user-images.githubusercontent.com/84711996/189540457-fea1a1dd-f281-4446-935d-86fa0fe543b1.png">
Step 4 - Check metrics with Grafana
- Browse to localhost:3001 with default account and password(admin/admin)
- Add data source for Prometheus
<img width="1440" alt="Screen Shot 2022-09-12 at 1 18 51 AM" src="https://user-images.githubusercontent.com/84711996/189540657-48dde7ec-c925-4a9e-9149-32787876c9e0.png">
- Import Dashboard with ID or JSON.(No results while search trino on grafan dashboard and get ID: 10866 while search presto but it is kind ot outdated)
<img width="1440" alt="Screen Shot 2022-09-12 at 1 18 51 AM" src="https://user-images.githubusercontent.com/84711996/189540699-11f4d72e-08de-4943-9334-efcded35284b.png">
<img width="1439" alt="Screen Shot 2022-09-12 at 1 22 56 AM" src="https://user-images.githubusercontent.com/84711996/189540790-b24cefc7-0d8d-46d5-a632-7b330a5ca7e0.png">
<img width="1440" alt="Screen Shot 2022-09-12 at 1 23 44 AM" src="https://user-images.githubusercontent.com/84711996/189540819-e6a3c7eb-5dca-4dff-b8a1-de9caea21b2a.png">

Step 5 - Confirm Logs from Kibana
- Browse to http://localhost:5601/app/management/kibana/indexPatterns and set up the index name pattern for Kibana. Specify fluentd-* to Index name or pattern and click Create.
<img width="1439" alt="Screen Shot 2022-09-12 at 1 24 35 AM" src="https://user-images.githubusercontent.com/84711996/189540858-a705bd82-76c2-44e1-9ed9-5973b3729061.png">

- Then, go to Discover tab to check the logs. As you can see, logs are properly collected into the Elasticsearch + Kibana, via Fluentd.
<img width="1438" alt="Screen Shot 2022-09-12 at 1 25 35 AM" src="https://user-images.githubusercontent.com/84711996/189540999-18a39b1b-9163-4536-b10a-5eb5d9218d0d.png">

Ref:
- https://trino.io/docs/current/admin/jmx.html
- https://github.com/prometheus/jmx_exporter
- https://docs.fluentd.org/container-deployment/docker-compose
- https://stackoverflow.com/questions/71120621/efk-system-is-build-on-docker-but-fluentd-cant-start-up
- https://www.josehisse.dev/blog/como-exportar-metricas-do-trino-para-o-prometheus

Some refs may need to be checked when implement with k8s:
- https://artifacthub.io/packages/helm/presto-loadbalancer/trino
- https://github.com/bluecatengineering/bunny-metrics
  

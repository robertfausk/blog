Vorgehen bei Fehler im Live-/Stage-System

1) Grafana 
2) Kibana https://www.elastic.co/de/products/kibana
3) Sentry https://sentry.io/

## Prometheus

- sammelt Metriken periodisch ein
- setzt timestamps selbst, da es die Abfrage selber macht
- uses OpenMetrics format https://openmetrics.io/
- provides API for Grafana
- deletes old data automatically
- consumes round about 100MB for 20 projects for 90 days (compares to ES which uses several GB for this case)
- very stable
- perhaps https://prometheus.projektmotor.de ?
- self monitoring for own target is possible (see example in next point)
- configured via scrapes which is simply prometheus.yml
doc: https://prometheus.io/docs/prometheus/latest/configuration/configuration/
```YML
# /etc/prometheus/prometheus.yml
global:
  scrape_interval: 15s
  scrape_timeout: 10s
  evaluation_interval: 15s

alerting:
  alertmanagers:
    - static_configs:
        - targets: []
      scheme: http
      timeout: 10s

scrape_configs:

  # Prometheus self-monitoring

- job_name: prometheus
    metrics_path: /metrics
    scheme: http
    static_configs:
      - targets:
          - localhost:9090
```

## Grafana

https://grafana.com/
- uses Prometheus, ES or several others as data source
- provides dashboard 
- provides alerting
-- notification possible via webhooks
- allows easy setting of markers (e.g. for feature toggle on, deployment, DDOS attack, cache deleted, sent >10000 newsletter)
-- deployment example: increased response time because there is more crawling from crawler. They crawl more cause there is a very low response time while page is in maintenance mode
- markers can also be set via command in Slack
- also usable with FritzBox via FritzBox exporter, e.g. https://github.com/ndecker/fritzbox_exporter
- bounce rate at mails
- easy extendable https://grafana.com/plugins
-- visualisation plugins
-- data sources
-- apps (also Zabbix)
- provides easy filtering and dividing
-- e.g. of users by 
--- being google
--- being from client company
--- being from own company
--- being a bot (which can also be filtered and divided)
-- e.g. "HTTP status code > 400 and only users"
-- e.g. backend sessions and frontend sessions
- all you can think of which is measurable

Concerning DSGVO:
when used for stability of application then it's OK

Using in example company:
- provide one dashboard per client
- client gets access and introduction to grafana for each panel and each defined treshold
- offer opt out because of data security reasons
- client also gets error mails
- bonus: much more transparency for client
- saves time of calling (or writing a mail) in error case
- write down added value in offer/contract for client



Conclusion:
In application there is more going wrong than unexpected.
Backgrounds are revealed.
Application is much more stable.
Much deeper understanding and more insights for developers.
Provides much more transparency for client.

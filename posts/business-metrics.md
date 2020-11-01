# Business metrics

## Intro use case

Approach when error in live/stage orrcurs:

1) Grafana 
2) Kibana
3) Sentry

## Kibana 

- https://www.elastic.co/de/products/kibana
- see all logs
- easy search and filter logs - YAY!

## Sentry

- https://sentry.io/
- see exceptions in code

## Prometheus

- crawls metrics periodically
- timeshift https://grafana.com/blog/categories/timeshift/
- sets timestamps on its own cause it's making requests on its own
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
  - notification possible via webhooks
  - make a call, send a sms etc.
  - create a ticket in redmine
- allows easy setting of markers (e.g. for feature toggle on, deployment, DDOS attack, cache deleted, sent >10000 newsletter)
  - deployment example: increased response time because there is more crawling from crawler. They crawl more cause there is a very low response time while page is in maintenance mode
- markers can also be set via command in Slack
- also usable with FritzBox via FritzBox exporter, e.g. https://github.com/ndecker/fritzbox_exporter
- bounce rate at mails
- easy extendable https://grafana.com/plugins
  - visualisation plugins
  - data sources
  - apps (also Zabbix)
- provides easy filtering and dividing
- examples:
  - divide app users by 
    - being google
    - being from client company
    - being from own company
    - being a bot (which can also be filtered and divided)
    - being screaming frog (https://www.screamingfrog.co.uk/seo-spider/ https://www.trafficmaxx.de/blog/seo/screaming-frog-fuer-einsteiger) crawler which can be bad configured like "do 5000 requests/s"
  - "HTTP status code > 400 and only users"
  - top 5 errors
  - backend sessions and frontend sessions
  - google page speed
  - all you can think of which is measurable
- practical hints:
  - connect mysql via replication or secured via iptables or at least basic auth
  - also monitor min load (or other min values)
  - use twilio for calls (1$/month) or use FritzBox (free but more initial work load)

Concerning DSGVO:
when used for stability of application then it's OK

Usage in example company:
- provide one dashboard per client
- client gets access and introduction to grafana for each panel and each defined treshold
- offer opt out because of data security reasons
- client also gets error mails
- bonus: much more transparency for client
- saves time of calling (or writing a mail) in error case
- write down added value in offer/contract for client

## Filebeat

- https://www.elastic.co/de/products/beats/filebeat
- also Heartbeat https://www.elastic.co/de/products/beats/heartbeat
- suggestion: use native in OS so that not everything is mounted
- mind:
  - max limit of request
  - goes through all logs on startup
1) getting streams; log output; need to provide path to log for import
2) preparse data if wished to
3) define target where to deliver to, e.g. deliver to Logstash

## Logstash

- https://www.elastic.co/products/logstash
- filter data
- change respective enrich data:
  - using "geoip"
  - using "grok", it's like regex; used for log lines of e.g. apache
https://grokconstructor.appspot.com/do/match or http://grokdebug.herokuapp.com/


## Conclusion

In application there is more going wrong than unexpected.
Backgrounds are revealed.
Application is much more stable.
Much deeper understanding and more insights for developers.
Provides much more transparency for client.

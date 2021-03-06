## Create a AWS IAM Sevice Account with IAM "AmazonEC2ReadOnlyAccess" permission
```
IAM role:
AmazonEC2ReadOnlyAccess
```

## Create couple of Ec2 Ubuntu16.04 Instance with following UserData Config.


### User data script:
```
#!/usr/bin/env bash
wget -O - https://raw.githubusercontent.com/amitvashisttech/prometheus-JPMC-24-Aug-2021/main/03-Node-Exporter/node-exporter.sh | bash
```



## Now Update the prometheus Config

/etc/prometheus/prometheus.yml
```
  - job_name: 'ec2_nodes'
    ec2_sd_configs:
      - region: eu-west-1
        access_key: PUT_THE_ACCESS_KEY_HERE
        secret_key: PUT_THE_SECRET_KEY_HERE
        port: 9100
    relabel_configs:
      - source_labels: [__meta_ec2_tag_Name]
        regex: Prometheus.*
        action: keep
      - source_labels: [__meta_ec2_public_ip]
        regex:  '(.*)'
        target_label: __address__
        replacement: '${1}:9100'
```

## Generate load:
```
dd if=/dev/zero of=/dev/null
```



## Query grafana:
```
100 - (avg by (instance) (irate(node_cpu_seconds_total{job="ec2_nodes",mode="idle"}[5m])) * 100)
```

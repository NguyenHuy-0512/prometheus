# Cài đặt kafka exporter:

cd /usr/kafka/libs/
wget https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.3.1/jmx_prometheus_javaagent-0.3.1.jar 
cd /usr/kafka/config/
wget https://raw.githubusercontent.com/prometheus/jmx_exporter/main/example_configs/kafka-0-8-2.yml

# Thêm systemd: kafka.service
# Location: /etc/systemd/system/kafka.service
[Unit]
Description=kafka

[Service]
Type=simple
Environment="JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64/"
Environment="KAFKA_OPTS=-javaagent:/usr/kafka/libs/jmx_prometheus_javaagent-0.3.1.jar=9091:/usr/prometheus/prometheus.yml"
Environment="KAFKA_OPTS=-javaagent:/usr/kafka/libs/jmx_prometheus_javaagent-0.3.1.jar=9091:/usr/kafka/config/kafka-0-8-2.yml"
ExecStart = /usr/kafka/bin/kafka-server-start.sh /usr/kafka/config/server.properties
ExecStop=/opt/kafka/bin/kafka-server-stop.sh
Restart=on-failure

[Install]
WantedBy = multi-user.target
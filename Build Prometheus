#!/bin/bash
#Version promotheus

PROMETHEUS_VERSION=$(curl -s https://api.github.com/repos/prometheus/prometheus/releases/latest | grep -Eo '[0-9]+\.[0-9]+\.[0-9]+' | head -n1)

#Telechargement package

wget https://github.com/prometheus/prometheus/releases/download/v$PROMETHEUS_VERSION/prometheus-$PROMETHEUS_VERSION.linux-amd64.tar.gz

#Extraction des packet

tar xvfz prometheus-$PROMETHEUS_VERSION.linux-amd64.tar.gz

#créations du repertoire et supprésions du package comprésées

rm prometheus-$PROMETHEUS_VERSION.linux-amd64.tar.gz
sudo mkdir -p /etc/prometheus /var/lib/prometheus
cd prometheus-$PROMETHEUS_VERSION.linux-amd64

#Déplacements des fichier utilisée

sudo mv prometheus promtool /usr/local/bin/
sudo mv prometheus.yml /etc/prometheus/prometheus.yml
sudo mv consoles/ console_libraries/ /etc/prometheus/

#créations du user et permission

sudo useradd -rs /bin/false prometheus
sudo chown -R prometheus: /etc/prometheus /var/lib/prometheus

#créations du service

cat <<EOL | sudo tee /etc/systemd/system/prometheus.service > /dev/null
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries \
    --web.listen-address=0.0.0.0:9090 \
    --web.enable-lifecycle \
    --log.level=info

[Install]
WantedBy=multi-user.target
EOL

#restart service
sudo systemctl daemon-reload

sudo systemctl enable prometheus --now
sudo systemctl status prometheus

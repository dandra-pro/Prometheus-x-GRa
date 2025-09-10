#!/bin/bash

NODE_EXPORTER_VERSION=$(curl -s https://api.github.com/repos/prometheus/node_exporter/releases/latest | grep -Eo '[0-9]+\.[0-9]+\.[0-9]+' | head -n1)
wget https://github.com/prometheus/node_exporter/releases/download/v$NODE_EXPORTER_VERSION/node_exporter-$NODE_EXPORTER_VERSION.linux-amd64.tar.gz

# Extract the Node Exporter package
tar xvfz node_exporter-*.tar.gz

# Move the Node Exporter binary to /usr/local/bin
sudo mv node_exporter-$NODE_EXPORTER_VERSION.linux-amd64/node_exporter /usr/local/bin/

# Clean up extracted files
rm -r node_exporter-$NODE_EXPORTER_VERSION.linux-amd64*

# Create the node_exporter user
sudo useradd -rs /bin/false node_exporter

# Create the Node Exporter systemd service file
cat <<EOL | sudo tee /etc/systemd/system/node_exporter.service > /dev/null
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
EOL

# Reload systemd to recognize the new service

sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter

# Check Node Exporter status
sudo systemctl status node_exporter

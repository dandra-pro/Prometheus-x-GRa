#!/bin/bash
GRAFANA_GPG_URL="https://apt.grafana.com/gpg.key"
GRAFANA_REPO="deb [signed-by=/usr/share/keyrings/grafana.key] https://apt.grafana.com stable main"
GRAFANA_VERSION="grafana"

# Update and install required packages
sudo apt-get update && sudo apt-get install -y apt-transport-https software-properties-common wget curl

# Import the Grafana GPG key
sudo wget -q -O /usr/share/keyrings/grafana.key "$GRAFANA_GPG_URL"

# Add the Grafana repository
echo "$GRAFANA_REPO" | sudo tee -a /etc/apt/sources.list.d/grafana.list

# Update the package list to include the Grafana repositor
sudo apt-get update

# Install Grafana
sudo apt-get install -y "$GRAFANA_VERSION"

# Reload systemd daemon to recognize the new Grafana service
sudo systemctl daemon-reload

# Enable and start the Grafana service
sudo systemctl enable grafana-server.service
sudo systemctl start grafana-server

# Check the status of the Grafana service
sudo systemctl status grafana-server

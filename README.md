# BPN - Bitcoin Personal Node

## Why I built this project
I was following the amazing Road to Knots guide and I wanted it to run on Docker

## Features

Bitcoin Knots : current best Bitcoin node implementation
Tor network : your Bitcoin node communicated anynomously on Tor
LND (Bitcoin Lightning Network) node
Electrum Personal Server (EPS) : your private bridge between your wallet and your node
Lightning terminal : monitor your lightning transactions

## Firewall configuration

The current docker-compose.yml file is designed to not modify your `iptables`. To ensure a maximal security, for an environment that only runs this project, add the following rules


```bash

# 1. Set default policies to deny incoming and allow outgoing
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Add your other policies here

# 3. Enable the firewall
sudo ufw enable
```
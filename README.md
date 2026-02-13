# BPN - Bitcoin Personal Node

## Why I built this project
I was following the amazing Road to Knots guide and I wanted it to run on Docker

This project is a Dockerized version of a Bitcoin node system from the project ![Road to Knots](https://scratch-knots.orangepill.ovh/). While I was quite amazed with this project, I wanted to make it run on Docker on my Raspberry Pi. This would help me to deploy it in seconds with **Docker compose**. 

## Features
The compose file contains the following features :

**Bitcoin Knots** : current best Bitcoin node implementation
**Tor network** : your Bitcoin node is anonymously discovered using Tor network; you don't need to open any port :)
**LND (Bitcoin Lightning Network)** : lightning node integrated for lightspeed transactions
**Electrum Personal Server (EPS)** : bridge your own node to your wallet to secure your transactions

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

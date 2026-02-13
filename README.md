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

## Bitcoin node configuration
This configuration file sets up a **pruned Bitcoin Knots node** running exclusively over **Tor**, with **RPC and ZMQ** enabled for external applications like **Lightning Network Daemon (LND)**.

---

### **1. Node Settings**
| Parameter | Value | Description |
|-----------|-------|-------------|
| `daemon` | `1` | Runs Bitcoin Knots as a **background daemon** (non-interactive). |
| `prune` | `5000` | Enables **pruning**, keeping only the last **5 GB** of blockchain data. Older blocks are deleted to save disk space. |
| `disablewallet` | `1` | **Disables the built-in Bitcoin wallet** (node is used for infrastructure, not personal funds). |
| `dbcache` | `2048` | Sets the **database cache to 2 GB** for improved performance (reduces disk I/O). |
| `txindex` | `0` | **Disables transaction indexing** (not needed for most use cases). |
| `onlynet` | `onion` | **Forces connections only over Tor** (no clearnet IPv4/IPv6). |

---

### **2. Tor Settings (Mandatory for Tor-Only)**
| Parameter | Value | Description |
|-----------|-------|-------------|
| `proxy` | `tor_proxy:9050` | Configures Bitcoin to use a **Tor SOCKS5 proxy** (likely a Docker service name). |
| `listenonion` | `1` | **Advertises the node as a Tor hidden service**, allowing other Tor nodes to connect. |

---

### **3. RPC/API Settings**
| Parameter | Value | Description |
|-----------|-------|-------------|
| `server` | `1` | **Enables the RPC server** for external applications (e.g., LND). |
| `rpcbind` | `0.0.0.0` | **Binds RPC to all network interfaces** (including Docker’s internal network). |
| `rpcport` | `8332` | Sets the **RPC port to 8332** (default for mainnet). |
| `rpcuser` | `your_rpc_username` | **RPC username** (should be strong and kept secret). |
| `rpcpassword` | `your_rpc_password` | **RPC password** (should be strong and kept secret). |

⚠️ **Security Note:**
- Exposing RPC to `0.0.0.0` is **risky**—ensure proper firewall rules restrict access to trusted IPs.
- Consider using **environment variables** for `rpcuser` and `rpcpassword` in production.

---

### **4. ZMQ (ZeroMQ) Settings (Required for LND)**
| Parameter | Value | Description |
|-----------|-------|-------------|
| `zmqpubrawblock` | `tcp://0.0.0.0:28332` | **Publishes new blocks** over ZMQ (used by LND). |
| `zmqpubrawtx` | `tcp://0.0.0.0:28333` | **Publishes new transactions** over ZMQ (used by LND). |

🔹 **Why ZMQ?**
- Provides **low-latency, high-performance** notifications for new blocks/transactions.
- **Required for Lightning Network nodes** to stay in sync.

---

### **Summary of Configuration**
| Feature | Setting |
|---------|---------|
| **Node Type** | Bitcoin Knots (pruned, Tor-only) |
| **Pruning** | 5 GB (keeps only recent blocks) |
| **Wallet** | Disabled (`disablewallet=1`) |
| **Network** | Tor-only (`onlynet=onion`) |
| **RPC** | Enabled (`server=1`), accessible on `0.0.0.0:8332` |
| **ZMQ** | Enabled for LND (`zmqpubrawblock`, `zmqpubrawtx`) |
| **Use Case** | Running a **Lightning Network node (LND)** over Tor with minimal disk usage. |

---

### **Security & Performance Considerations**
✅ **Pros:**
- **Privacy:** Tor-only mode enhances anonymity.
- **Storage Efficiency:** Pruning reduces disk usage.
- **Lightning-Ready:** ZMQ enables LND integration.

⚠️ **Caveats:**
- **Pruning means you can’t serve old blocks** to other nodes.
- **RPC exposure (`0.0.0.0`)** requires strict firewall rules.
- **Tor may slow down initial sync** compared to clearnet.

This setup is **ideal for Lightning Network operators** who need a **private, low-storage Bitcoin node**.



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

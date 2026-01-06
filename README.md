Note: This installation Step can be done in Virtual machine or on containers. I manuall created 4 containers using incus.

Use below command inside your VM
Command: ssh-keygen -t ed25519 -C "router"
By default, this creates two files:

~/.ssh/id_ed25519 (private key)

~/.ssh/id_ed25519.pub (public key)

Copy the public key to your container:
Add the contents of your public key (id_ed25519.pub) to the container’s ~/.ssh/authorized_keys

Change Permission:
chown -R ubuntu:ubuntu /home/ubuntu/.ssh
chmod 700 /home/ubuntu/.ssh
chmod 600 /home/ubuntu/.ssh/authorized_keys

# Ansible Role: prometheus

This Ansible role installs and configures **Prometheus** on a Linux host using the
official binary release.  
It configures Prometheus as a **systemd service**, deploys alert rules, and
includes native support for **SNMP monitoring via the Prometheus SNMP Exporter**.

The role is designed to integrate cleanly with:
- SNMP Exporter
- Alertmanager
- SNMPv3-enabled devices (switches / routers)

---

## ✅ Features

- Installs Prometheus (versioned binary release)
- Installs `promtool`
- Creates configuration and data directories
- Deploys `prometheus.yml`
- Deploys alert rules (`alerts.yml`)
- Configures SNMP scrape jobs
- Integrates with Alertmanager
- Runs Prometheus as a systemd service
- Idempotent and safe to re-run

---

## ✅ Supported Platforms

- Ubuntu 20.04+
- Debian-based systems
- VMs and containers (Incus / LXD)

---

## 📦 Requirements

- SSH access to the target host
- `sudo` privileges
- Ansible ≥ 2.12
- SNMP Exporter reachable from Prometheus
- Alertmanager reachable (optional but recommended)

---

## 🔧 Role Variables

Variables can be defined in `roles/prometheus/vars/main.yml` or overridden via
inventory or `group_vars`.

### `roles/prometheus/vars/main.yml`

| Variable | Description | Example |
|-------|------------|--------|
| `prometheus_version` | Prometheus version | `2.54.1` |
| `snmp_target` | SNMP-enabled device IP | `10.12.242.150` |
| `snmp_exporter` | SNMP Exporter address | `10.12.242.206:9116` |
| `alertmanager_target` | Alertmanager address | `10.12.242.213:9093` |

---

## 📁 Inventory Example

```ini
[prometheus]
prometheus ansible_host=10.12.242.205 ansible_user=ubuntu

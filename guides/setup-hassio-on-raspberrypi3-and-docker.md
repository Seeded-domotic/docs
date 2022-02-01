# Setup HASSIO on Raspberrypi3 and Docker

This guide was created on 01/02/2022 following several guides found over the Hassio forums.

## 📜 Prerequisites
- Raspberrypi3
- Raspbian GNU/Linux 11 (bullseye) installed (the official Raspberry OS)

Note: it should work with other Debian-based distributions, but it hasn't been tested yet.

## 🧑🏻‍💻 Configuration

- Login as a superuser:
```bash
sudo su
```

- Install dependencies:
```bash
apt update && sudo apt full-upgrade -y

apt install jq wget curl udisks2 libglib2.0-bin network-manager dbus apparmor-utils -y
```

- Install and configure Docker:
```bash
curl -fsSL get.docker.com | sh

usermod -aG docker pi
```

- Install OS-Agent:
```bash
wget https://github.com/home-assistant/os-agent/releases/download/1.2.2/os-agent_1.2.2_linux_armv7.deb

dpkg -i os-agent_1.2.2_linux_armv7.deb
```

- Install Home Assistant Supervised:
```bash
wget https://github.com/home-assistant/supervised-installer/releases/latest/download/homeassistant-supervised.deb

dpkg -i homeassistant-supervised.deb
# Select 'raspberrypi3' as an architecture when prompted
```

## 🧰 After installation
The installation process is slow, it will take a while to HASSIO to be up and running.

HASSIO relates on seven Docker containers (`docker ps` command) to work and they should be appearing on the following order:
  - hassio_supervisor
  - hassio_cli
  - hassio_dns
  - hassio_audio
  - hassio_observer
  - hassio_multicast
  - homeassistant

After the last one (`homeassistant`) is up, the **Home Assistant** webpage should be accesible on the 8123 of the raspberrypi IP, e.g http://192.168.0.200:8123.

All HASSIO files are located at `/usr/share/hassio/`
## 🧹 Uninstalling HASSIO

```
systemctl stop hassio-supervisor.service
systemctl stop hassio-apparmor.service
systemctl disable hassio-supervisor.service
systemctl disable hassio-apparmor.service
systemctl daemon-reload

rm /etc/systemd/system/hassio-*

docker rm hassio_supervisor hassio_cli hassio_dns hassio_audio hassio_observer hassio_multicast homeassistant
```

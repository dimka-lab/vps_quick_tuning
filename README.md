# vps_quick_tuning
Quick primitive setting VPS Ubuntu

```bash
# Update packages
sudo apt update

# Base tools
sudo apt install -y nano htop curl wget net-tools ethtool

# Fix Hyper-V / network offloading issues
# sudo ethtool -K eth0 tx off rx off tso off gso off gro off

# Create user with sudo
adduser master
usermod -aG sudo master

sudo mkdir -p /home/master
sudo chown master:master /home/master
sudo chmod 700 /home/master

# Fail2ban
sudo apt install fail2ban -y

sudo nano /etc/fail2ban/jail.local
```

```
[DEFAULT]
bantime = 12h
findtime = 10m
maxretry = 5
backend = systemd

[sshd]
enabled = true
port = 6749
filter = sshd
```

```bash
sudo systemctl enable fail2ban
sudo systemctl restart fail2ban
sudo fail2ban-client status sshd

# Проверить открытые порты
sudo ss -tulpn

# Firewall (безопасный порядок)

# Очистить
sudo iptables -F
sudo iptables -X

# Разрешить базовое
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Разрешить SSH (старый порт)
sudo iptables -A INPUT -p tcp --dport 6749 -j ACCEPT

# (опционально) HTTP/HTTPS
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Установить политики (после разрешений)
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# Проверить
sudo iptables -nvL

# Настроить SSH
sudo nano /etc/ssh/sshd_config
```

```
Port 6749
PermitRootLogin no
PasswordAuthentication yes
```

```bash
# Перезапустить SSH
sudo systemctl restart ssh

# Проверить вход (НОВОЕ подключение!)
ssh master@SERVER_IP -p 6749

# Если работает — можно удалить старый порт
sudo iptables -D INPUT -p tcp --dport 22 -j ACCEPT

# Сохранить iptables
sudo apt install iptables-persistent -y
sudo netfilter-persistent save

# Shadowsocks (если нужен)
sudo apt install shadowsocks-libev -y

sudo nano /etc/shadowsocks-libev/config.json
```

```json
{
    "server": "0.0.0.0",
    "server_port": 34782,
    "password": "STRONG_PASSWORD_HERE",
    "timeout": 300,
    "method": "chacha20-ietf-poly1305",
    "mode": "tcp_and_udp"
}
```

```bash
# Открыть порт shadowsocks
sudo iptables -A INPUT -p tcp --dport 34782 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 34782 -j ACCEPT

sudo systemctl enable shadowsocks-libev
sudo systemctl restart shadowsocks-libev

# Пересохранить правила
sudo netfilter-persistent save
```

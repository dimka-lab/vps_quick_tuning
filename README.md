# vps_quick_tuning
Quick primitive setting VPS Ubuntu

Update pacakges
### 
```bash
sudo apt update
```
Создадим пользователя с sudo-привилегиями
```bash
adduser master
usermod -aG sudo master
```

```bash
sudo mkdir -p /home/master
sudo chown master:master /home/master
sudo chmod 700 /home/master
```

Защита от брутфорса (fail2ban)
```bash
sudo apt install fail2ban -y
```

```bash
sudo nano /etc/fail2ban/jail.local
```

```
[DEFAULT]
bantime = 12h
findtime = 10m
maxretry = 4
bantime.increment = true
bantime.factor = 2

[sshd]
enabled = true
port = 6749
```

```bash
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
sudo systemctl status fail2ban
```

```bash
sudo fail2ban-client status
```

```bash
sudo fail2ban-client status sshd
```

Проверить открытые порты
```bash
sudo ss -tulpn
```

Сменить порт ssh
```bash
sudo nano /etc/ssh/sshd_config
```

```config
Port 2222
PermitRootLogin no
PasswordAuthentication no
```

Настроить iptables (минимально безопасный набор)

Очистить старые правила:
```bash
sudo iptables -F
sudo iptables -X
```

Политики по умолчанию:
```bash
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT
```

Разрешить локальный интерфейс:
```bash
sudo iptables -A INPUT -i lo -j ACCEPT
```

Разрешить уже установленные соединения:
```bash
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

Разрешить новый SSH-порт:
```bash
sudo iptables -A INPUT -p tcp --dport 2222 -m conntrack --ctstate NEW -j ACCEPT
```

если есть HTTP/HTTPS
```bash
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

Проверить
```bash
sudo iptables -nvL
```

Перезапустить ssh
```bash
sudo systemctl restart ssh
```

```bash
ssh master@SERVER_IP -p 2222
```

Сохранить правила iptables (иначе пропадут после reboot)

```bash
sudo apt install iptables-persistent -y
sudo netfilter-persistent save
```


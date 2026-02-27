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
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

```bash
sudo fail2ban-client status
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





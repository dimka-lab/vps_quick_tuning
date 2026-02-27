```bash
sudo apt update
sudo apt install shadowsocks-libev -y
```

```bash
sudo nano /etc/shadowsocks-libev/config.json
```

```bash
{
    "server":"0.0.0.0",
    "server_port":34782,
    "password":"STRONG_PASSWORD_HERE",
    "timeout":300,
    "method":"chacha20-ietf-poly1305",
    "mode":"tcp_and_udp",
    "fast_open":false,
    "nameserver":"1.1.1.1"
}
```

```bash
sudo iptables -A INPUT -p tcp --dport 34782 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 34782 -j ACCEPT
sudo netfilter-persistent save
```

```bash
sudo systemctl enable shadowsocks-libev
sudo systemctl restart shadowsocks-libev
sudo systemctl status shadowsocks-libev
```

На сервере
```bash
curl -O https://raw.githubusercontent.com/angristan/wireguard-install/master/wireguard-install.sh
chmod +x wireguard-install.sh
./wireguard-install.sh
```

It is also available in /home/baklan/wg0-client-node_a.conf

```bash
cat /home/baklan/wg0-client-node_a.conf
```

Ставим wireguard чистый на клиенте
```bash
sudo apt install wireguard -y
```

```bash
wg --version
```
На клиенте
```bash
sudo nano /etc/wireguard/wg0.conf
```
и вставить содержимое wg0-client-node_a.conf

```
[Interface]
PrivateKey = <key>
Address = 10.66.66.9/32, fd42:42:42::9/128

[Peer]
PublicKey = <public key>
PresharedKey = <pre shared key>
Endpoint = <ip_сервера>:55289
AllowedIPs = 10.66.66.0/24, fd42:42:42::/64
PersistentKeepalive = 25
```

```bash
sudo chmod 600 /etc/wireguard/wg0.conf
```

```bash
sudo systemctl enable wg-quick@wg0
sudo systemctl start wg-quick@wg0
```

```bash
sudo wg
```

```bash
ping 10.66.66.1
```

Отдельная таблица маршрутизации
```bash
echo "200 wgtable" | sudo tee -a /etc/iproute2/rt_tables
```

в этой таблице default через wg0
```bash
sudo ip route add default dev wg0 table wgtable
```

Маркируем исходящий трафик, где source port = 34782 Shadowsocks
```bash
sudo iptables -t mangle -A OUTPUT -p tcp --sport 34782 -j MARK --set-mark 1
sudo iptables -t mangle -A OUTPUT -p udp --sport 34782 -j MARK --set-mark 1
```

правило маршрутизации по марке
```bash
sudo ip rule add fwmark 1 table wgtable
```


SSH → напрямую
Трафик от Shadowsocks → через wg0 → WireGuard сервер



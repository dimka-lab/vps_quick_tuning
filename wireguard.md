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

На клиенте (тоже сервере)
```bash
sudo nano /etc/wireguard/wg0.conf
```
и вставить содержимое wg0-client-node_a.conf

Ставим wireguard чистый на клиенте
```bash
sudo apt install wireguard -y
```

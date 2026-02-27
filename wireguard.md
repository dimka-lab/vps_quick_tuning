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






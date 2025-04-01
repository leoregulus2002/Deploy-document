## 下载kibana
```shell
wget https://artifacts.elastic.co/downloads/kibana/kibana-8.17.4-linux-x86_64.tar.gz
tar -xvf kibana-8.17.4-linux-x86_64.tar.gz
cd kibana-8.17.4
```
## 生成密钥
```
bin/kibana-encryption-keys generate
```
## 配置kibana.yml
```
server.host: "0.0.0.0"
elasticsearch.username: kibana_admin
elasticsearch.password: password
xpack.encryptedSavedObjects.encryptionKey: 3841d404f704ef8a10eeb8f1d865e1c5
xpack.reporting.encryptionKey: 22eaa4542d0af7cabd2e0d72af71cbd0
xpack.security.encryptionKey: f589d276ef418807d10907cad6b72dff
```
## 启动kibana

```
sudo -u kibana ./bin/kibana
```

## 可选注册到systemctl

```
[Unit]
Description=Kibana
After=network.target

[Service]
ExecStart=/home/user/kibana/kibana-8.17.4/bin/kibana
WorkingDirectory=/home/user/kibana/kibana-8.17.4/
Restart=always
User=kibana
Group=kibana
Environment=ENV_VAR=value

[Install]
WantedBy=multi-user.target
```

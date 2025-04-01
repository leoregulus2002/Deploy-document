## 下载Elasticsearch
```shell
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.17.4-linux-x86_64.tar.gz
tar -xvf elasticsearch-8.17.4-linux-x86_64.tar.gz
cd elasticsearch-8.17.4
```

## 创建elasticsearch用户
```shell
sudo useradd -r -s /bin/false elasticsearch
```
## 设置文件夹权限
```shell
sudo chown -R elasticsearch:elasticsearch /path/to/elasticsearch
```
## 禁用 HTTPS 强制使用 HTTP
```shell
vim config/elasticsearch.yml

xpack.security.http.ssl.enabled: false
```

#  以 elasticsearch 用户身份启动 Elasticsearch
```shell
sudo -u elasticsearch /path/to/elasticsearch/bin/elasticsearch
```

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ Elasticsearch security features have been automatically configured!

✅ Authentication is enabled and cluster connections are encrypted.

ℹ️  Password for the **elastic** user (reset with `bin/elasticsearch-reset-password -u elastic`):

  **DVr4h-O=s4Z34I2Uyhw0**

ℹ️  HTTP CA certificate SHA-256 fingerprint:

  **745ff0743bd1f13b06bc22651ae2c48c68a0977f84f2013e4d666f1c037b1d2c**

ℹ️  Configure Kibana to use this cluster:

• Run Kibana and click the configuration link in the terminal when Kibana starts.

• Copy the following enrollment token and paste it into Kibana in your browser (valid for the next 30 minutes):

  **eyJ2ZXIiOiI4LjE0LjAiLCJhZHIiOlsiMTkyLjE2OC4wLjM6OTIwMCJdLCJmZ3IiOiI3NDVmZjA3NDNiZDFmMTNiMDZiYzIyNjUxYWUyYzQ4YzY4YTA5NzdmODRmMjAxM2U0ZDY2NmYxYzAzN2IxZDJjIiwia2V5IjoiS0VZQzc1VUJtdUxjdHpJTzRkN1c6UF9QTllNd0RTTVdpYVZyQ0ppVGpPZyJ9**

ℹ️  Configure other nodes to join this cluster:

• On this node:

  ⁃ Create an enrollment token with `bin/elasticsearch-create-enrollment-token -s node`.

  ⁃ Uncomment the **transport.host** setting at the end of **config/elasticsearch.yml**.

  ⁃ Restart Elasticsearch.

• On other nodes:

  ⁃ Start Elasticsearch with `bin/elasticsearch --enrollment-token <token>`, using the enrollment token that you generated.


## 修改密码
```
curl -X POST "http://localhost:9200/_security/user/elastic/_password" -u "elastic:current_password" -H "Content-Type: application/json" -d '{
  "password" : "new_password"
}'
```
## 创建kibana用户配置角色kibana_system
```
  curl -X PUT "http://localhost:9200/_security/user/kibana_admin" -H "Content-Type: application/json" -d'
{
  "password" : "password",
  "roles" : [ "kibana_system" ],
  "full_name" : "kibana_admin"
}' -u elastic:DVr4h-O=s4Z34I2Uyhw0
```
## 验证用户是否能访问es
```
curl -X GET "http://localhost:9200/_cluster/health?pretty" -u "kibana_admin:password"
```

## 【可选】注册到systemctl

```
[Unit]
Description=Elasticsearch
After=network.target

[Service]
ExecStart=/home/user/elasticsearch/elasticsearch-8.17.4/bin/elasticsearch
WorkingDirectory=/home/user/elasticsearch/elasticsearch-8.17.4
Restart=always
User=elasticsearch
Group=elasticsearch
Environment=ENV_VAR=value

[Install]
WantedBy=multi-user.target
```

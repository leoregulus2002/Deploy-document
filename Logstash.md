## 下载logstash
```shell
wget https://artifacts.elastic.co/downloads/logstash/logstash-8.17.4-linux-x86_64.tar.gz
tar -xvf logstash-8.17.4-linux-x86_64.tar.gz
cd logstash-8.17.4
mkdir conf
vim logstash.conf
```
## 配置读取log文件的路径以及输出到es
```shell
input {
  file {
    path => "/var/log/syslog"  # 设置你要读取的日志文件路径
    start_position => "beginning"  # 从日志文件的开头开始读取（可选）
    sincedb_path => "/dev/null"  # 防止记录已读取的位置（调试用，可以在生产环境中设置为一个文件路径）
  }
}

output {
  elasticsearch {
    hosts => ["http://localhost:9200"]
    index => "logstash-%{+YYYY.MM.dd}"
    user => "your_username"          # 设置用户名
    password => "your_password"
  }
}
```
## 启动logstash
```shell
./bin/logstash -f ./conf/logstash.conf
```

## 【可选】注册到systemctl

```
[Unit]
Description=Logstash
After=network.target

[Service]
ExecStart=/home/user/logstash/logstash-8.17.4/bin/logstash -f /home/user/logstash/logstash-8.17.4/conf/logstash.conf
WorkingDirectory=/home/user/logstash/logstash-8.17.4
Restart=always
User=root
Group=root
Environment=ENV_VAR=value

[Install]
WantedBy=multi-user.target
```

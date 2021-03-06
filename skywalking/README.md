# skywalking（8900）

### 官网
[https://skywalking.apache.org](https://skywalking.apache.org)

### 下载相应版本的 SkyWalking APM
[https://skywalking.apache.org/downloads/](https://skywalking.apache.org/downloads/)

### 本地部署

##### 修改 SkyWalking 存储配置文件（默认：H2）
修改 /opt/apache-skywalking-apm-6.6.0/config/application.yml 文件中 mysql/elasticsearch 对应存储的配置

##### 修改 SkyWalking Webapp 端口（默认：8080）
修改 /opt/apache-skywalking-apm-6.6.0/webapp/webapp.yml 文件中 server.port 端口号

##### 启动 SkyWalking 的 OAP 服务跟 Webapp 服务
```shell script
bin/startup.sh
```

### docker方式

##### 拉取 SkyWalking OAP 服务
```shell script
docker pull apache/skywalking-oap-server:6.6.0-es7
```

##### 拉取 SkyWalking Webapp 服务
```shell script
docker pull apache/skywalking-ui:6.6.0
```

##### 启动 SkyWalking OAP 服务
```shell script
docker run -d \
-e SW_STORAGE=mysql \
-e SW_JDBC_URL=jdbc:mysql://localhost:3306/skywalking \
-e SW_DATA_SOURCE_USER=lion \
-e SW_DATA_SOURCE_PASSWORD=lion \
--name skywalking-oap-server \
--restart always \
apache/skywalking-oap-server:6.6.0-es7
```

##### 启动 SkyWalking Webapp 服务
```shell script
docker run -d \
-p 8900:8080 \
-e SW_OAP_ADDRESS=skywalking-oap-server:12800 \
--name skywalking-oap-server \
--restart always \
apache/skywalking-ui:6.6.0
```

### 成功启动后进入 dashboard 界面
[http://localhost:8900](http://localhost:8900)

### IDEA 部署探针
- 打开 Run -> Edit Configurations...
- 在服务启动类的 Environment -> VM options 增加以下内容
```shell script
-javaagent:/opt/apache-skywalking-apm-6.6.0/agent/skywalking-agent.jar
-Dskywalking.agent.service_name=lion-demo
-Dskywalking.collector.backend_service=localhost:11800
```

### Java 命令行启动方式
```shell script
java -javaagent:/opt/apache-skywalking-apm-6.6.0/agent/skywalking-agent.jar -Dskywalking.agent.service_name=lion-demo -Dskywalking.collector.backend_service=localhost:11800 -jar app.jar
```

### 设置存储方式
修改 /opt/apache-skywalking-apm-6.6.0/config/application.yml 文件中的相关配置即可

*注：使用 MySQL 存储时，需下载 MySQL 驱动包拷贝到 oap-libs 目录下*
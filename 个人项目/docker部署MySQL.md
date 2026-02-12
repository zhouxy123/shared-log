新建目录，例如mysql-docker，进入
创建docker-compose.yml文件：
```yaml
services:
  mysql:
    image: mysql:8.0
    container_name: mysql8
    restart: unless-stopped
    ports:
      - "3306:3306" // 注意，若3306端口已被占用，可考虑改为 - "3307:3306"
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
      TZ: "Asia/Singapore"
    command: >
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_0900_ai_ci
      --default-time-zone=+08:00
      --sql-mode=STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
    volumes:
      - mysql_data:/var/lib/mysql
      - ./init:/docker-entrypoint-initdb.d
volumes:
  mysql_data:

```

创建.env文件，配置库名、账户、密码：
```env
MYSQL_ROOT_PASSWORD=xxx
MYSQL_DATABASE=xxx
MYSQL_USER=zhouxy
MYSQL_PASSWORD=xxx
```

创建/启动容器：
```
docker compose up -d
```


连接： (端口号、用户名自己改)
```
mysql -h 127.0.0.1 -P 3307 -u zhouxy -p
```
之后输入之前设定的MYSQL_PASSWORD





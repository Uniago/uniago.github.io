---
title: Tiny Tiny RSS搭建
date: 2023-11-14 09:00:21 +0800
categories: ['Personal', 'RSS']
tags: ['RSS阅读', '服务搭建']
---

## 通过docker-compose部署TTRSS

> [参考Aweson TTRSS](https://ttrss.henry.wang/zh/#%E9%80%9A%E8%BF%87-docker-%E9%83%A8%E7%BD%B2)

### 1. 拉取docker-compose.yaml
```bash
# 创建 ttrss目录并进入
mkdir ttrss && cd ttrss

# 使用curl下载ttrss的docker-compose配置文件至服务器
curl -fLo docker-compose.yml https://raw.githubusercontent.com/HenryQW/Awesome-TTRSS/main/docker-compose.yml
```

### 2. 修改配置文件
```yaml
version: "3"
services:
  service.rss:
    image: wangqiru/ttrss:latest
    container_name: ttrss
    ports:
      - 181:80
    environment:
      - SELF_URL_PATH=https://ttrss.uniago.cn:181/ # please change to your own domain
      - DB_PASS=uniago-ttrss # use the same password defined in `database.postgres`
      - PUID=1000
      - PGID=1000
    volumes:
      - feed-icons:/var/www/feed-icons/
    networks:
      - public_access
      - service_only
      - database_only
    stdin_open: true
    tty: true
    restart: always

  service.mercury: # set Mercury Parser API endpoint to `service.mercury:3000` on TTRSS plugin setting page
    image: wangqiru/mercury-parser-api:latest
    container_name: mercury
    networks:
      - public_access
      - service_only
    restart: always

  service.opencc: # set OpenCC API endpoint to `service.opencc:3000` on TTRSS plugin setting page
    image: wangqiru/opencc-api-server:latest
    container_name: opencc
    environment:
      - NODE_ENV=production
    networks:
      - service_only
    restart: always

  database.postgres:
    image: postgres:13-alpine
    container_name: postgres
    environment:
      - POSTGRES_PASSWORD=uniago-ttrss # feel free to change the password
    volumes:
      - ~/postgres/data/:/var/lib/postgresql/data # persist postgres data to ~/postgres/data/ on the host
    networks:
      - database_only
    restart: always

  # utility.watchtower:
  #   container_name: watchtower
  #   image: containrrr/watchtower:latest
  #   volumes:
  #     - /var/run/docker.sock:/var/run/docker.sock
  #   environment:
  #     - WATCHTOWER_CLEANUP=true
  #     - WATCHTOWER_POLL_INTERVAL=86400
  #   restart: always

volumes:
  feed-icons:

networks:
  public_access: # Provide the access for ttrss UI
  service_only: # Provide the communication network between services only
    internal: true
  database_only: # Provide the communication between ttrss and database only
    internal: true
```
需要修改三处: SELF_URL_PATH,DB_PASS,POSTGRES_PASSWORD

### 3. 部署到docker
```bash
docker-compose up -d
```

### 4. 反向代理, 并添加tls证书
```tex
localhost:181 -> ttrss.uniago.cn
```

### 5. 访问
```tex
https://ttrss.uniago.cn
```
若没有域名,可通过ip+端口方式登录

### 6. 登录并修改密码
- 默认账户：`admin` 
- 密码：`password`

### 7. 在设置中开启插件
 - 启用 `mercury-fulltext` 插件, 填入Mercury Parser API地址:`service.mercury:3000`
 - 启用`Fever API`, 插件设置中设置密码,在客户端中使用 `https://ttrss.uniago.cn/plugins/fever` 作为服务器地址
 - 启用`opencc` 插件,填入OpenCC API 地址:`service.opencc:3000`

### 忘记密码

> [参考挖站否](https://wzfou.com/ttrss-docker/)

连接数据库
```bash
#连接数据库
docker exec -ti postgres psql -U postgres

#切换数据库（可选）
\c
ttrss
```

关闭OTP 或 重置密码
```sql
# 关闭OTP
UPDATE ttrss_users SET otp_enabled = false WHERE login = 'admin'

# 恢复密码为password
	UPDATE ttrss_users SET pwd_hash = 'SHA1:5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8', salt = '', otp_enabled = false WHERE login = 'admin'
```

## 搭建RSSHub

>[参考RSSHub官网文档](https://docs.rsshub.app/zh/install#docker-%E9%83%A8%E7%BD%B2)

### 1. 下载 RSSHub 镜像

```bash
docker pull diygod/rsshub
```

### 2. 运行
```bash
docker run -d --name rsshub -p 1200:1200 diygod/rsshub
```

### 3.下载浏览器插件
[RSSHub Radar](https://github.com/DIYgod/RSSHub-Radar)

### 4.配置插件
自定义RSSHub域名
```tex
https://rss.uniago.cn
```

配置在页面上的`订阅到TTRSS按钮`, 开启`Tiny Tiny RSS`
```tex
# 设置为rsshub的域名
https://ttrss.uniago.cn
```
## 客户端

|平台| 客户端|
|--|--|
|IOS|Fiery Feeds<br /> Readably <br /> Reeder|
|Android| Feedme<br /> TTRSS-Reader|
|Mac/Win|直接在ttrss网页浏览|

---
layout: page
title:  "Mysql Local Server"
---

Run mysql server in docker container:

```bash
docker run -d --name local-mysql -e MYSQL_ROOT_PASSWORD=password -p 3306:3306 mysql
```
Connect to mysql server:

```text
Host: 127.0.0.1
Port: 3306
User: root
Password: password
```
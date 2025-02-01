---
layout: page
title:  "Mysql Local Server"
---

Create a docker-compose file:

```yml
version: '3.8'
services:
  mysql:
    image: mysql:8.0
    container_name: my_mysql
    environment:
      MYSQL_ROOT_PASSWORD: your_root_password
      MYSQL_DATABASE: your_database_name
      MYSQL_USER: your_username
      MYSQL_PASSWORD: your_password
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

Start mysql server:

```bash
docker-compose up -d
```

Verify mysql server is running:

```bash
docker ps
```

Connect to mysql server:

```text
Host: localhost
Port: 3306
User: your_username (or root)
Password: your_password (or your_root_password)
Database: your_database_name
```
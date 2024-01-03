---
layout: post
title:  "Mysql installation on Ubuntu"
date:   2023-01-04 00:00:00 +0100
categories: sysadmin
---
```bash
sudo apt install mysql-server
```

```bash
sudo systemctl start mysql.service
```

```bash
sudo mysql
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
exit;
```

```bash
mysql_secure_installation
```

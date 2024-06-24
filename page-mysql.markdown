---
layout: page
title:  "02 - Mysql"
---

## INSTALLAZIONE

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

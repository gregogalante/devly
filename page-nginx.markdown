---
layout: page
title:  "01 - Nginx"
---

## CONFIGURAZIONE SITO WEB STATICO

```nginx
server {
  listen 80;
  server_name example.com; # dominio applicazione
  root /home/deploy/applications/APP_PATH/public; # directory applicazione
  index index.html;
}
```

## CONFIGURAZIONE CON UPSTREAM

```nginx
upstream unique_upstream_name {
  server 127.0.0.1:8001; # localhost with application port
}

server {
  listen 80; # server host with 80 port
  server_name servername.com; # custom application domain
  location / {
    proxy_pass  http://unique_upstream_name;
    proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
    proxy_redirect off;
    proxy_buffering off;
    proxy_set_header        Host            $host;
    proxy_set_header        X-Real-IP       $remote_addr;
    proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
    # proxy_set_header        X-Forwarded-Proto https; # activate in case of problem with https/http
  }
}
```

## REDIRECT DA HTTP A HTTPS

```nginx
server {
  listen 80;
  server_name example.com;
  return 301 https://$server_name$request_uri;
}
```

## CERTIFICATI SSL TRAMITE LET'S ENCRYPT

### Installare certbot

```bash
sudo apt install certbot python3-certbot-nginx
```

### Creare un certificato con certbot

```bash
sudo certbot --nginx -d example.com
```

Dopo l'esecuzione la configurazione di nginx verrà aggiornata automaticamente con i certificati SSL.

## CERTIFICATI SSL TRAMITE LET'S ENCRYPT (BACKUP)

### Installare certbot

```bash
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot
```

### Creare un certificato con certbot

```bash
sudo certbot certonly -a webroot --webroot-path=/APP_PUBLIC_PATH -d yourdomain.here
```

NOTE: /APP_PUBLIC_PATH corrisponde alla path pubblica dell’applicazione. Durante questa fase certbot crea un file all’interno della directory per verificare la proprietà del dominio.

### Aggiornare/rinnovare i certificati scaduti con certbot

```bash
sudo certbot renew --dry-run
```

### Impostare un cron di auto-aggiornamento dei certificati

```bash
sudo nano /etc/cron.d/certbot-custom

37 02 * * * root certbot -q renew --pre-hook="systemctl stop nginx" --post-hook="systemctl start nginx"
```

### Impostare il certificato su una configurazione nginx

```nginx
server {
  # ...
  listen 443 ssl; # add listener on 443 path
  ssl_certificate /etc/letsencrypt/live/yourdomain.here/fullchain.pem; # add ssl certificate
  ssl_certificate_key /etc/letsencrypt/live/yourdomain.here/privkey.pem; # add ssl certificate key
  # ...
}
```

### Creare/aggiornare un certificato senza utilizzo di un webserver

```bash
sudo certbot certonly --cert-name yourdomain.here
```

Successivamente scegliere l’opzione 1 per creare un certificato.<br>
NOTE: Per eseguire questa operazione è necessario che la porta 80 del server sia aperta.

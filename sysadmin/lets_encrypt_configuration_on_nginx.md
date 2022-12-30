# LET'S ENCRYPT CONFIGURATION ON NGINX

## Installare certbot

```bash
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot
```

## Creare un certificato con certbot

```bash
sudo certbot certonly -a webroot --webroot-path=/APP_PUBLIC_PATH -d yourdomain.here
```

NOTE: /APP_PUBLIC_PATH corrisponde alla path pubblica dell’applicazione. Durante questa fase certbot crea un file all’interno della directory per verificare la proprietà del dominio.

## Aggiornare/rinnovare i certificati scaduti con certbot

```bash
sudo certbot renew --dry-run
```

## Impostare un cron di auto-aggiornamento dei certificati

```bash
sudo nano /etc/cron.d/certbot-custom

37 02 * * * root certbot -q renew --pre-hook="systemctl stop nginx" --post-hook="systemctl start nginx"
```

## Impostare il certificato su una configurazione nginx

```txt
server {
  listen 80;
  listen 443 ssl; # add listener on 443 path
  ssl_certificate /etc/letsencrypt/live/yourdomain.here/fullchain.pem; # add ssl certificate
  ssl_certificate_key /etc/letsencrypt/live/yourdomain.here/privkey.pem; # add ssl certificate key
  server_name example.com;
  passenger_enabled on;
  root /home/deploy/applications/APP_PATH/public;
  index index.html;
}
```

# Configurazione di Nginx su server Ubuntu 18+

Dei seguito alcuni verranno presentati alcuni esempi delle principali tipologie di applicativi configurabili nel file /etc/nginx/site-available/default o su altri file nella stessa directory.

## SITO WEB STATICO

```txt
server {
  listen 80;
  server_name example.com; # dominio applicazione
  root /home/deploy/applications/APP_PATH/public; # directory applicazione
  index index.html;
}
```

## CREARE UN UPSTREAM PER GESTIRE DIVERSI APPLICATIVI IN ASCOLTO SULLA PORTA 80

Quando il server deve gestire diverse applicazioni sulla porta 80 la soluzione consiste nel creare diversi proxy che, dato il dominio, eseguono un upstream di applicazioni in ascolto su altre porte.

Se, per esempio, avete una applicazione sulla porta 8001, per girare su di essa tutte le richieste con dominio servername.com basta inserire i segenti codici:

```txt
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
  }
}
```

## CREARE UN REDIRECT DA HTTP A HTTPS

```txt
server {
  listen 80;
  server_name example.com;
  return 301 https://$server_name$request_uri;
}
```
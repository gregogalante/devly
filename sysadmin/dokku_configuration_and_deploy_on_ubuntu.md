# Dokku configuration and deploy on Ubuntu

Guida su come configurare e deployare un'applicazione su un server Ubuntu con Dokku.

## Configurazione Dokku

### Installazione Dokku su Ubuntu Digital Ocean

Su Digital Ocean creare un droplet con Ubuntu 22.04 e installare Dokku selezionandolo direttamente dal marketplace.
Impostare la connessione SSH alla macchina tramite chiave pubblica/privata.

### Configurazione Dokku

Ottenere la propria chiave pubblica:

```sh
cat ~/.ssh/id_rsa.pub
```

Aggiungere la chiave pubblica del proprio computer al server Dokku:

```sh
mkdir public_keys
nano public_keys/your-computer-name.pub
dokku ssh-keys:add KEY_NAME public_keys/your-computer-name.pub
```

Installare plugin per Postgres:

```sh
sudo dokku plugin:install https://github.com/dokku/dokku-postgres.git
```

Installare plugin per Let's Encrypt:

```sh
sudo dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git
dokku letsencrypt:set --global email your-email@your.domain.com
dokku letsencrypt:cron-job --add
```

### Creazione applicazione

Creare l'applicazione:

```sh
dokku apps:create your-app-name
```

Creare il database:

```sh
dokku postgres:create your-app-name
dokku postgres:link your-app-name your-app-name
```

Configurare il dominio:

```sh
dokku domains:set your-app-name your-app-name.com
dokku letsencrypt:enable your-app-name
```

### Deploy applicazione

Aggiungere il repository remoto:

```sh
git remote add dokku dokku@server.ip:our-app-name
git push dokku main:master
```

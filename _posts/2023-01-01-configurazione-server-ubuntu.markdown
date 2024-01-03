---
layout: post
title:  "🇮🇹 Configurazione server Ubuntu"
date:   2023-01-01 00:00:00 +0100
categories: sysadmin
---
## CREAZIONE DI UN UTENTE DEPLOY

Per motivi di sicurezza è una buona pratica utilizzare un utenza diversa da root per gestire le attività di deploy degli applicativi all’interno del server.

Per creare un utente deploy bisogna accedere con l’utenza root ed eseguire i seguenti comandi:

```bash
sudo adduser deploy
gpasswd -a deploy sudo
```

Alla richiesta di inserire una password per l’utente si consiglia di generare una password complessa.

Una volta completata l’installazione uscire dal sistema operativo e rientrare utilizzando l’utenza deploy appena creata.

## INSTALLAZIONE DELLE DIPENDENZE DEL SISTEMA

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install -y git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev libffi-dev
```

## METTERE IN SICUREZZA IL SERVER

Per evitare accessi indesiderati tramite brute force, bisogna almeno impostare un sistema di ban dei tentativi falliti di connessione. Per farlo è consigliata l’installazione di fail2ban.

```bash
sudo apt-get install fail2ban
```

Eventualmente personalizzare la configurazione di default per rendere il sistema più sicuro; per farlo bisogna seguire i seguenti comandi:

```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
```

## INSTALLARE NODE.JS

```bash
curl -sL https://deb.nodesource.com/setup_18.x -o nodesource_setup.sh
sudo bash nodesource_setup.sh
sudo apt install nodejs
```

## INSTALLARE NGINX

```bash
sudo apt-get install nginx
```

## INSTALLARE PM2

```bash
npm install -g pm2
```

## ATTIVARE ELIMINAZIONE LOG PM2

```bash
crontab -e
```

```txt
0 0 * * * rm -rf /home/deploy/.pm2/logs/*
```



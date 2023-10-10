# Installazione di Nginx e PM2 su server Ubuntu 18+

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

## INSTALLARE IL LINGUAGGIO DI PROGRAMMAZIONE RUBY (FACOLTATIVO)

Per l’installazione di Ruby si consiglia di utilizzare RVM seguendo la guida presente nel sito ufficiale: https://rvm.io.

Eventualmente seguire i seguenti comandi:

```bash
sudo apt install gnupg2
gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
curl -sSL https://get.rvm.io | bash -s stable
```

## INSTALLARE NODE.JS (FACOLTATIVO)

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


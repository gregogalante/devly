---
layout: post
title:  "Import OpenStreetMap data locally"
date:   2024-01-06 00:00:00 +0100
categories: dev
---

I'm using [Docker](https://www.docker.com/) to import OpenStreetMap data locally.
I'm using [osm2pgsql](https://github.com/ingmapping/docker-osm2pgsql) docker image and [postgis-osm](https://github.com/ingmapping/docker-postgis-osm) docker image.

## Steps

Pull docker image:

```bash
docker pull ingmapping/postgis-osm
```

Start posgresql with postgis docker image :

```bash
docker run --name postgis-osm -d -p  5432:5432 ingmapping/postgis-osm
```

Create a directory and download .osm.pbf file:

```bash
mkdir ~/osm
cd ~/osm
wget https://download.geofabrik.de/europe/faroe-islands-latest.osm.pbf # Faroe Islands (small size for testing)
wget https://ftp.fau.de/osm-planet/pbf/planet-latest.osm.pbf # Planet (big size)
```

Run import:

```bash
docker run -i -t --rm --link postgis-osm:pg -v ~/osm:/osm ingmapping/osm2pgsql -c 'osm2pgsql --create --slim --cache 2000 --number-processes 4 --database $PG_ENV_POSTGRES_DB --username $PG_ENV_POSTGRES_USER --host pg --port $PG_PORT_5432_TCP_PORT /osm/faroe-islands-latest.osm.pbf'
```

Now you can connect to local postgresql database and read data:

- Host: 127.0.0.1
- Port: 5432
- Username: postgres
- Password: password

**IMPORTANT**: Run on a machine with a lot of DISK SPACE (min 1TB)
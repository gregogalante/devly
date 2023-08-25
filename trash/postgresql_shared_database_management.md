# PostgreSQL shared database management

Guida su come gestire database multipli condivisi su Postgre.
Questa guida è stata applicata in particolare su un servizio DB postgre Digital Ocean utilizzato su diversi progetti.
Ad ogni progetto è stato applicato un utente e database diverso aventi lo stesso nome.

## METTERE IN SICUREZZA GLI ACCESSI DATABASE/UTENTI

Dopo aver creato gli utenti e i database eseguire il seguente script python:

```py
import psycopg2

conn = psycopg2.connect(
  host="CAMBIAMI",
  database="CAMBIAMI",
  user="CAMBIAMI",
  password="CAMBIAMI",
  port="CAMBIAMI",
  sslmode="require"
)

excluded_db_names = ['postgres', 'template0', 'template1', '_dodb', 'defaultdb']

cur = conn.cursor()
query_database_names = "SELECT datname FROM pg_database;"
cur.execute(query_database_names)
database_names = cur.fetchall()
cur.close()

for database_name in database_names:
  db_name = database_name[0]
  if db_name in excluded_db_names:
    continue

  # check user exists with same db name
  query_user_exists = "SELECT 1 FROM pg_roles WHERE rolname='{}';".format(db_name)
  cur = conn.cursor()
  cur.execute(query_user_exists)
  user_exists = cur.fetchone()
  cur.close()
  if user_exists == None:
    print("User {} does not exist".format(db_name))
    continue
  
  # revoke all privileges to user to all databases
  for database_revoke_name in database_names:
    db_revoke_name = database_revoke_name[0]
    query_revoke_privileges = "REVOKE ALL PRIVILEGES ON DATABASE \"{}\" FROM \"{}\";".format(db_revoke_name, db_name)
    cur = conn.cursor()
    cur.execute(query_revoke_privileges)
    conn.commit()
    print("Revoked privileges for user {} to database {}".format(db_name, db_revoke_name))
    cur.close()

  # add privileges to user to only it's database 
  query_grant_privileges = "GRANT ALL PRIVILEGES ON DATABASE \"{}\" TO \"{}\";".format(db_name, db_name)
  cur = conn.cursor()
  cur.execute(query_grant_privileges)
  conn.commit()
  print("Granted privileges for user {} to database {}".format(db_name, db_name))
  cur.close()

conn.close()
```

## RESET ID AUTO-INCRMENTALI

Nel caso di database importati da altre strutture dati è importante effettuare il reset degli ID auto-incrementali attraverso l'esecuzione del seguente script python:

```py
import psycopg2

conn = psycopg2.connect(
  host="CAMBIAMI",
  database="CAMBIAMI",
  user="CAMBIAMI",
  password="CAMBIAMI",
  port="CAMBIAMI",
  sslmode="require"
)

cur = conn.cursor()
query_table_names = "SELECT table_name FROM information_schema.tables WHERE table_schema='public' AND table_type='BASE TABLE';"
cur.execute(query_table_names)
table_names = cur.fetchall()
cur.close()

for table_name in table_names:
  cur = conn.cursor()
  try:
    query_reset_id_counter = "SELECT setval('{}_id_seq', (SELECT MAX(id) + 1 FROM {}));".format(table_name[0], table_name[0])
    cur.execute(query_reset_id_counter)
    conn.commit()
    print("Reset id counter for table {}".format(table_name[0]))
  except Exception as e:
    conn.rollback()
    print(e)
  finally:
    cur.close()

conn.close()
```
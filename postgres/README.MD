## PostgreSQL
PostgreSQL, often simply Postgres, is an open source object-relational database management system (ORDBMS) with an emphasis on extensibility and standards compliance

* [Basic commands](#basic-commands)
* [Kill all existing connections](#kill-existing-connections)
* [Trust local connections](#trust-local-connections)
* [Find data storage location](#find-data-storage-location)

### Basic commands
#### Connect to a server
```bash
psql -h localhost -p 5432 -U postgres
```

#### List all databases
```bash
psql -h localhost -p 5432 -U postgres --list

\list
```
#### Get given database
```bash
psql -h localhost -p 5432 -U postgres --list | grep given_database
```

#### Quit PSQL CLI
```bash
\q
```

### Kill existing connections
A database can't be deleted if a client is still connected to it, hence we might need to kill all current connections :
```sql
SELECT 
    pg_terminate_backend(pid) 
FROM 
    pg_stat_activity 
WHERE 
    pid <> pg_backend_pid()
    AND datname = 'database_name';
```

### Trust local connections
Local connections can be trusted in a secure environment - disabling credentials in this case can also allow resetting postgres password. For this purpose, modify `pg_hba.conf` in :
* Windows : `C:\Program Files\PostgreSQL\9.X\data\`
* Linux (usually) : `/etc/postgresql/9.X/main/`

### Find data storage location
As data location varies from a distribution to another, it can be queried through psql :

```sql
SELECT s.name, s.setting FROM pg_settings WHERE s.name='data_directory';
```

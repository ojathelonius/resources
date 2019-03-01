## PostgreSQL
PostgreSQL, often simply Postgres, is an open source object-relational database management system (ORDBMS) with an emphasis on extensibility and standards compliance

* [Basic commands](#basic-commands)
* [Kill all existing connections](#kill-existing-connections)

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

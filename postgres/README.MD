## PostgreSQL
PostgreSQL, often simply Postgres, is an open source object-relational database management system (ORDBMS) with an emphasis on extensibility and standards compliance

* [Basic commands](#basic-commands)
* [Kill all existing connections](#kill-existing-connections)
* [Trust local connections](#trust-local-connections)
* [Find data storage location](#find-data-storage-location)
* [PostGIS : getting started](#postgis-getting-started)

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
#### List all users
```bash
\du
```
#### Give user SUPERUSER
```sql
ALTER USER user WITH SUPERUSER;
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
### PostGIS : getting started

#### Installation
On Windows, PostGIS can be installed through the **Application StackBuilder** that comes with Postgres.

On CentOS 7, run the following :
```shell
yum -y install epel-release
sudo rpm -ivh http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum install postgis2_94 postgis2_94-client
```

#### Database configuration
Add the PostGIS plugin to the database :
```sql
CREATE EXTENSION postgis;
CREATE EXTENSION postgis_topology;
```

#### Data processing

First add a geometry column :
```sql
ALTER TABLE feature ADD COLUMN geom geometry;
COMMENT ON COLUMN feature.geom IS 'Geometry';
```
Then process its data using PostGIS functions (e.g. below with a single point feature) :
```sql
UPDATE feature feat SET geom = ST_Point(feat.lon, feat.lat);
```

The result is then best viewed using [QGIS](https://www.qgis.org/) or [Geoserver](http://geoserver.org/).

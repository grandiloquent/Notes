# PostgreSQL Administration Cookbook, 9.5/9.6 Edition
 
## Connecting to the PostgreSQL server

```

psql postgresql://myuser:mypasswd@myhost:5432/mydb

```

```

SELECT current_database();

```

```

SELECT current_user;

```

```

SELECT inet_server_addr(), inet_server_port();

```

```

SELECT version();

```

```

postgres=# conninfo

```

## Enabling access for network/remote users

```

        listen_addresses = '*'

```

```

        # TYPE   DATABASE   USER        CIDR-ADDRESS   METHOD 
        Host      all        all          0.0.0.0/0       md5

```

```

listen_addresses = '*'

```

## Using graphical administration tools

## Using the psql query and scripting tool

```

psql -h myhost -p 5432 -d mydb -U myuser 
psql postgresql://myuser@myhost:5432/mydb

```

```

$ psql -c "SELECT current_time"
     timetz
-----------------
 18:48:32.484+01
(1 row)

```

```

$ psql -f examples.sql

```

```

SET
SET
SET
SET
SET
SET
DROP SCHEMA
CREATE SCHEMA
SET
SET
SET
CREATE TABLE
CREATE TABLE
COPY 5
COPY 3

```

```

$ psql -c "SELECT current_time" –f examples.sql -c "SELECT current_time"
     timetz
-----------------
 18:52:15.287+01
(1 row)
   ...output removed for clarity...
     timetz
-----------------
 18:58:23.554+01
(1 row)

```

```

$ psql
postgres=#

```

```

postgres=# help

```

```

postgres=# quit

```

```

postgres=# h DELETE
Command:     DELETE
Description: delete rows of a table
Syntax:
[ WITH [ RECURSIVE ] with_query [, ...] ]
DELETE FROM [ ONLY ] table [ [ AS ] alias ]
    [ USING usinglist ]
    [ WHERE condition | WHERE CURRENT OF cursor_name ]
    [ RETURNING * | output_expression [ AS output_name ] [,]]

```

```

-- This is a single-line comment

```

```

/*
 * Multi-line comment
*/

```

## Changing your password securely

```

password

```

```

ALTER USER postgres PASSWORD ' md53175bce1d3201d16594cebf9d7eb3f9d';

```

```

ALTER USER myuser PASSWORD 'secret'

```

## Avoiding hardcoding your password

```

host:port:dbname:user:password

```

```

myhost:5432:postgres:sriggs:moresecure

```

## Using a connection service file

```

[dbservice1] 
host=postgres1 
port=5432 
dbname=postgres

```

```

service=dbservice1 user=sriggs

```

## Troubleshooting a failed connection

```

            log_connections = on
            log_disconnections = on

```

## Exploring the Database

## Introduction

## What version is the server?

```

postgres # SELECT version();

```

```

PostgreSQL 9.5.4 on x86_64-pc-linux-gnu, compiled by gcc (Debian 4.9.2-10) 4.9.2, 64-bit

```

```

bash # psql --version
psql (PostgreSQL) 9.5.4

```

```

bash # cat $PGDATA/PG_VERSION

```

## What is the server uptime?

```

postgres=# SELECT date_trunc('second', 
current_timestamp - pg_postmaster_start_time()) as uptime;

```

```

     uptime 
--------------------------------------
 2 days 02:48:04

```

```

postgres=# SELECT pg_postmaster_start_time(); 
   pg_postmaster_start_time 
----------------------------------------------
2016-09-01 19:37:41.389134+00

```

```

postgres=# SELECT current_timestamp - pg_postmaster_start_time(); 
       ?column? 
--------------------------------------------------------
2 days 02:50:02.23939

```

```

postgres=# SELECT date_trunc('second', 
 current_timestamp - pg_postmaster_start_time()) as uptime; 
     uptime 
----------------------------
2 days 02:51:18

```

## Locating the database server files

```

export PGROOT=/var/lib/pgsql/ 
export PGRELEASE=9.6 
export PGSERVERNAME=mamba 
export PGDATA=$PGROOT/$PGRELEASE/$PGSERVERNAME

```

## Locating the database server's message log

```

2016-09-01 19:37:41 GMT [2507-1] LOG:  database system was shut down at 2016-09-01 19:37:38 GMT
2016-09-01 19:37:41 GMT [2507-2] LOG:  MultiXact member wraparound protections are now enabled
2016-09-01 19:37:41 GMT [2506-1] LOG:  database system is ready to accept connections
2016-09-01 19:37:41 GMT [2511-1] LOG:  autovacuum launcher started

```

## Locating the database's system identifier

```

pg_controldata <data-directory> | grep "system identifier"
Database system identifier:           5558338346489861223

```

```

/usr/lib/postgresql/9.5/bin/pg_controldata $PGDATA

```

```

pg_control version number:            942
Catalog version number:               201510051
Database system identifier:           6320137769675861859
Database cluster state:               in production
pg_control last modified:             Thu 01 Sep 2016 08:17:41 PM GMT
Latest checkpoint location:           0/19000728
Prior checkpoint location:            0/19000648
Latest checkpoint's REDO location:    0/190006F0
Latest checkpoint's REDO WAL file:    000000010000000000000019
Latest checkpoint's TimeLineID:       1
Latest checkpoint's PrevTimeLineID:   1
Latest checkpoint's full_page_writes: on
Latest checkpoint's NextXID:          0/703
Latest checkpoint's NextOID:          24978
Latest checkpoint's NextMultiXactId:  1
Latest checkpoint's NextMultiOffset:  0
Latest checkpoint's oldestXID:        616
Latest checkpoint's oldestXID's DB:   1
Latest checkpoint's oldestActiveXID:  703
Latest checkpoint's oldestMultiXid:   1
Latest checkpoint's oldestMulti's DB: 1
Latest checkpoint's oldestCommitTsXid:0
Latest checkpoint's newestCommitTsXid:0
Time of latest checkpoint:            Thu 01 Sep 2016 08:17:41 PM GMT
Fake LSN counter for unlogged rels:   0/1
Minimum recovery ending location:     0/0
Min recovery ending loc's timeline:   0
Backup start location:                0/0
Backup end location:                  0/0
End-of-backup record required:        no
wal_level setting:                    hot_standby
wal_log_hints setting:                on
max_connections setting:              100
max_worker_processes setting:         8
max_prepared_xacts setting:           0
max_locks_per_xact setting:           64
track_commit_timestamp setting:       off
Maximum data alignment:               8
Database block size:                  8192
Blocks per segment of large relation: 131072
WAL block size:                       8192
Bytes per WAL segment:                16777216
Maximum length of identifiers:        64
Maximum columns in an index:          32
Maximum size of a TOAST chunk:        1996
Size of a large-object chunk:         2048
Date/time type storage:               64-bit integers
Float4 argument passing:              by value
Float8 argument passing:              by value
Data page checksum version:           0

```

## Listing databases on this database server

```

bash $ psql -l
                               List of databases
   Name    | Owner  | Encoding |   Collate   |    Ctype    | Access privileges
-----------+--------+----------+-------------+-------------+-------------------
 postgres  | sriggs | UTF8     | en_GB.UTF-8 | en_GB.UTF-8 |
 template0 | sriggs | UTF8     | en_GB.UTF-8 | en_GB.UTF-8 | =c/sriggs        +
           |        |          |             |             | sriggs=CTc/sriggs
 template1 | sriggs | UTF8     | en_GB.UTF-8 | en_GB.UTF-8 | =c/sriggs        +
           |        |          |             |             | sriggs=CTc/sriggs
(3 rows)

```

```

postgres=# select datname from pg_database;
datname
-----------
template1
template0
postgres
(3 rows)

```

```

CREATE DATABASE my_database;

```

```

bash $ createdb my_database

```

```

postgres=# x
postgres=# select * from pg_database;
-[ RECORD 1 ]-+------------------------------
datname       | template1
datdba        | 10
encoding      | 6
datcollate    | en_GB.UTF-8
datctype      | en_GB.UTF-8
datistemplate | t
datallowconn  | t
datconnlimit  | -1
datlastsysoid | 11620
datfrozenxid  | 644
dattablespace | 1663
datacl        | {=c/sriggs,sriggs=CTc/sriggs}
-[ RECORD 2 ]-+------------------------------
datname       | template0
datdba        | 10
encoding      | 6
datcollate    | en_GB.UTF-8
datctype      | en_GB.UTF-8
datistemplate | t
datallowconn  | f
datconnlimit  | -1
datlastsysoid | 11620
datfrozenxid  | 644
dattablespace | 1663
datacl        | {=c/sriggs,sriggs=CTc/sriggs}
-[ RECORD 3 ]-+------------------------------
datname       | postgres
datdba        | 10
encoding      | 6
datcollate    | en_GB.UTF-8
datctype      | en_GB.UTF-8
datistemplate | f
datallowconn  | t
datconnlimit  | -1
datlastsysoid | 11620
datfrozenxid  | 644
dattablespace | 1663
datacl        |

```

## How many tables are there in a database?

```

SELECT count(*) FROM information_schema.tables
WHERE table_schema NOT IN ('information_schema',
'pg_catalog');

```

```

postgres@ebony:~/9.5/main$ psql -c "d"
        List of relations
 Schema |   Name   | Type  |  Owner  
--------+----------+-------+----------
 public | accounts | table | postgres
 public | branches | table | postgres

```

## How much disk space does a database use?

```

SELECT pg_database_size(current_database());

```

```

SELECT sum(pg_database_size(datname)) from pg_database;

```

## How much disk space does a table use?

```

postgres=# select pg_relation_size('pgbench_accounts');

```

```

pg_relation_size
------------------
           13582336
(1 row)

```

```

postgres=# select pg_total_relation_size('pgbench_accounts');

```

```

pg_total_relation_size
------------------------
         15425536
(1 row)

```

```

postgres=# dt+ pgbench_accounts
                        List of relations
 Schema |       Name       | Type  | Owner  | Size  | Description
--------+------------------+-------+--------+-------+-------------
 gianni | pgbench_accounts | table | gianni | 13 MB |
(1 row)

```

```

SELECT pg_size_pretty(pg_relation_size('pgbench_accounts'));

```

```

pg_size_pretty
----------------
13 MB
(1 row)

```

## Which are my biggest tables?

```

SELECT table_name
,pg_relation_size(table_schema || '.' || table_name) as size
FROM information_schema.tables
WHERE table_schema NOT IN ('information_schema', 'pg_catalog')
ORDER BY size DESC
LIMIT 10;

```

## How many rows are there in a table?

```

SELECT count(*) FROM table;

```

```

postgres=# select count(*) from orders;
count
â”€â”€â”€â”€â”€â”€â”€
     345
(1 row)

```

## Quickly estimating the number of rows in a table

```

SELECT (CASE WHEN reltuples > 0 THEN
pg_relation_size(oid)*reltuples/(8192*relpages)
ELSE 0
END)::bigint AS estimated_row_count
FROM pg_class
WHERE oid = 'mytable'::regclass;

```

```

estimated_count
---------------------
             293
(1 row)

```

```

        SELECT reltablespace, relfilenode FROM pg_class  
        WHERE oid = 'mytable'::regclass;

```

```

        SELECT oid as databaseid FROM pg_database  
        WHERE datname = current_database();

```

```

$PGDATADIR/base/{databaseid}/{relfilenode}*

```

```

$PGDATADIR/pg_tblspc/{reltablespace}/
{databaseid}/{relfilenode}*

```

## Function 1 - estimating the number of rows

```

CREATE OR REPLACE FUNCTION estimated_row_count(text) 
RETURNS bigint 
LANGUAGE sql 
AS $$ 
SELECT (CASE WHEN reltuples > 0 THEN 
              pg_relation_size($1)*reltuples/(8192*relpages) 
             ELSE 0 
             END)::bigint 
FROM pg_class 
WHERE oid = $1::regclass; 
$$;

```

## Function 2 - computing the size of a table without locks

```

CREATE OR REPLACE FUNCTION pg_relation_size_nolock(tablename regclass) 
RETURNS BIGINT 
LANGUAGE plpgsql 
AS $$ 
DECLARE 
    classoutput RECORD; 
    tsid        INTEGER; 
    rid         INTEGER; 
    dbid        INTEGER; 
    filepath    TEXT; 
    filename    TEXT; 
    datadir     TEXT; 
    i           INTEGER := 0; 
    tablesize   BIGINT; 
BEGIN 
    -- 
    -- Get data directory 
    -- 
    EXECUTE 'SHOW data_directory' INTO datadir; 
    -- 
    -- Get relfilenode and reltablespace 
    -- 
    SELECT 
     reltablespace as tsid 
    ,relfilenode as rid 
    INTO classoutput 
    FROM pg_class 
    WHERE oid = tablename 
    AND relkind = 'r'; 
    -- 
    -- Throw an error if we can't find the tablename specified 
    -- 
    IF NOT FOUND THEN 
      RAISE EXCEPTION 'tablename % not found', tablename; 
    END IF; 
    tsid := classoutput.tsid; 
    rid := classoutput.rid; 
    -- 
    -- Get the database object identifier (oid) 
    -- 
    SELECT oid INTO dbid 
    FROM pg_database 
    WHERE datname = current_database(); 
    -- 
    -- Use some internals knowledge to set the filepath 
    -- 
    IF tsid = 0 THEN 
  filepath := datadir || '/base/' || dbid || '/' || rid; 
    ELSE 
  filepath := datadir || '/pg_tblspc/' || tsid || '/' 
              || dbid || '/' || rid; 
    END IF; 
    -- 
    -- Look for the first file. Report if missing 
    -- 
    SELECT (pg_stat_file(filepath)).size 
    INTO tablesize; 
    -- 
    -- Sum the sizes of additional files, if any 
    -- 
    WHILE FOUND LOOP 
  i := i + 1; 
  filename := filepath || '.' || i; 
        -- 
        -- pg_stat_file returns ERROR if it cannot see file 
        -- so we must trap the error and exit loop 
        -- 
      BEGIN 
      SELECT tablesize + (pg_stat_file(filename)).size 
          INTO tablesize; 
      EXCEPTION 
           WHEN OTHERS THEN 
          EXIT; 
      END; 
    END LOOP; 
    RETURN tablesize; 
END; 
$$;

```

## Listing extensions in this database

```

cookbook=> SELECT * FROM pg_extension;

```

```

-[ RECORD 1 ]--+--------
extname        | plpgsql
extowner       | 10
extnamespace   | 11
extrelocatable | f
extversion     | 1.0
extconfig      |
extcondition   |

```

## Understanding object dependencies

```

CREATE TABLE orders (
orderid integer PRIMARY KEY
);
CREATE TABLE orderlines (
orderid integer
,lineid smallint
,PRIMARY KEY (orderid, lineid)
);

```

```

ALTER TABLE orderlines ADD FOREIGN KEY (orderid)
REFERENCES orders (orderid);

```

```

DROP TABLE orders;
ERROR: cannot drop table orders because other objects depend on it
DETAIL: constraint orderlines_orderid_fkey on table orderlines depends on table orders
HINT: Use DROP ... CASCADE to drop the dependent objects too.

```

```

d+ orders

```

```

SELECT * FROM pg_constraint
WHERE confrelid = 'orders'::regclass;

```

```

DROP TABLE orders;

```

## Configuration

## Introduction

## Reading the fine manual

## Planning a new database

## Changing parameters in your programs

```

SET work_mem = '16MB';

```

```

SET LOCAL work_mem = '16MB';

```

```

RESET work_mem;

```

```

RESET ALL;

```

```

SET work_mem = '16MB';

```

```

postgres=# SELECT name, setting, reset_val, source
                       FROM pg_settings WHERE source = 'session'; 
   name   | setting | reset_val | source   
----------+---------+-----------+--------- 
 work_mem | 16384   | 1024      | session

```

```

RESET work_mem;

```

```

  name   | setting | reset_val | source   
---------+---------+-----------+--------- 
work_mem | 1024    | 1024      | default

```

```

SET LOCAL work_mem = '16MB';

```

```

WARNING:  SET LOCAL can only be used in transaction blocks
SET

```

```

postgres=# SELECT name, setting, reset_val, source 
                       FROM pg_settings WHERE source = ‘session’;
   name   | setting | reset_val | source 
----------+---------+-----------+---------
 work_mem |  1024   | 1024      | session

```

```

BEGIN; 
SET LOCAL work_mem = '16MB';

```

```

postgres=# SELECT name, setting, reset_val, source
                       FROM pg_settings WHERE source = 'session'; 
   name   | setting | reset_val | source   
----------+---------+-----------+--------- 
 work_mem | 16384   | 1024      | session

```

## Finding the current configuration settings

```

postgres=# SHOW work_mem;

```

```

work_mem
----------
1MB
(1 row)

```

```

postgres=# x 
Expanded display is on. 
postgres=# SELECT * FROM pg_settings WHERE name = 'work_mem'; 
[ RECORD 1 ] -------------------------------------------------------- 
name       | work_mem 
setting    | 1024 
unit       | kB 
category   | Resource Usage / Memory 
short_desc | Sets the maximum memory to be used for query workspaces. 
extra_desc | This much memory can be used by each internal sort operation and hash table before switching to temporary disk files. 
context    | user 
vartype    | integer 
source     | default 
min_val    | 64 
max_val    | 2147483647 
enumvals   | 
boot_val   | 1024 
reset_val  | 1024 
sourcefile | 
sourceline |

```

```

postgres=# SHOW config_file;

```

```

               config_file
------------------------------------------
 /etc/postgresql/9.4/main/postgresql.conf
(1 row)

```

## Which parameters are at non-default settings?

```

postgres=# SELECT name, source, setting 
                        FROM pg_settings  
                        WHERE source != 'default' 
                        AND source != 'override' 
                        ORDER by 2, 1;

```

```

            name            |        source        |      setting 
----------------------------+----------------------+----------------- 
 application_name           | client               | psql 
 client_encoding            | client               | UTF8 
 DateStyle                  | configuration file   | ISO, DMY 
 default_text_search_config | configuration file   | pg_catalog.english 
 dynamic_shared_memory_type | configuration file   | posix 
 lc_messages                | configuration file   | en_GB.UTF-8 
 lc_monetary                | configuration file   | en_GB.UTF-8 
 lc_numeric                 | configuration file   | en_GB.UTF-8 
 lc_time                    | configuration file   | en_GB.UTF-8 
 log_timezone               | configuration file   | Europe/Rome 
 max_connections            | configuration file   | 100 
 port                       | configuration file   | 5460 
 shared_buffers             | configuration file   | 16384 
 TimeZone                   | configuration file   | Europe/Rome 
 max_stack_depth            | environment variable | 2048

```

## Updating the parameter file

```

pg_ctl reload

```

```

pg_ctlcluster 9.6 main reload

```

```

sudo systemctl reload postgresql@9.6-main 

```

```

pg_ctl restart

```

```

pg_ctlcluster 9.6 main restart

```

```

sudo systemctl restart postgresql@9.6-main

```

```

ALTER SYSTEM SET shared_buffers = '1GB';

```

## Setting parameters for particular groups of users

```

    ALTER DATABASE saas
 SET configuration_parameter = value1;

```

```

    ALTER ROLE Simon
 SET configuration_parameter = value2;

```

```

    ALTER ROLE Simon
 IN DATABASE saas
 SET configuration_parameter = value3;

```

## The basic server configuration checklist

```

kernel.shmmax=value

```

## Adding an external module to PostgreSQL

```

    apt-get install postgresql-9.2-debversion

```

```

    rpm -ivh orafce-3.0.1-1.pg84.rhel5.x86_64.rpm

```

```

make install

```

## Using an installed module

```

CREATE EXTENSION myextname;

```

```

CREATE EXTENSION dblink;

```

## Managing installed extensions

```

    postgres=# x on
Expanded display is on.
postgres=# SELECT *
postgres-# FROM pg_available_extensions
postgres-# ORDER BY name;
-[ RECORD 1 ]-----+--------------------------------------------------
name              | adminpack
default_version   | 1.0
installed_version |
comment           | administrative functions for PostgreSQL
-[ RECORD 2 ]-----+--------------------------------------------------
name              | autoinc
default_version   | 1.0
installed_version |
comment           | functions for autoincrementing fields
(...)

```

```

    -[ RECORD 10 ]----+--------------------------------------------------
name              | dblink
default_version   | 1.0
installed_version | 1.0
comment           | connect to other PostgreSQL databases from within a database

```

```

postgres=# x off 
Expanded display is off. 
postgres=# dx+ dblink 
                      Objects in extension "dblink" 
                           Object Description 
--------------------------------------------------------------------- 
 function dblink_build_sql_delete(text,int2vector,integer,text[]) 
 function dblink_build_sql_insert(text,int2vector,integer,text[],text[]) 
 function dblink_build_sql_update(text,int2vector,integer,text[],text[]) 
 function dblink_cancel_query(text) 
 function dblink_close(text) 
 function dblink_close(text,boolean) 
 function dblink_close(text,text) 
(...)

```

```

postgres=# DROP FUNCTION dblink_close(text); 
ERROR:  cannot drop function dblink_close(text) because extension dblink requires it 
HINT:  You can drop extension dblink instead.

```

```

postgres=# CREATE EXTENSION earthdistance; 
ERROR:  required extension "cube" is not installed 
postgres=# CREATE EXTENSION cube; 
CREATE EXTENSION 
postgres=# CREATE EXTENSION earthdistance; 
CREATE EXTENSION

```

```

postgres=# DROP EXTENSION cube; 
ERROR:  cannot drop extension cube because other objects depend on it 
DETAIL:  extension earthdistance depends on extension cube 
HINT:  Use DROP ... CASCADE to drop the dependent objects too. 
postgres=# DROP EXTENSION cube CASCADE; 
NOTICE:  drop cascades to extension earthdistance 
DROP EXTENSION

```

```

    dx+ db*

```

```

    ALTER EXTENSION myext UPDATE TO '1.1';

```

```

postgres=# CREATE EXTENSION earthdistance CASCADE; 
NOTICE:  installing required extension "cube" 
CREATE EXTENSION

```

## Server Control

## Introduction

## Starting the database server manually

```

systemctl start SERVICEUNIT

```

```

        postgresql@RELEASE-CLUSTERNAME 

```

```

        sudo systemctl start postgresql

```

```

        sudo systemctl start postgresql@9.6-main

```

```

        sudo systemctl start postgresql

```

```

        sudo systemctl start postgresql-9.6

```

```

        pg_ctlcluster 9.6 main start

```

```

        service postgresql start

```

```

        net start postgres

```

```

        pg_ctl -D $PGDATA start

```

```

sudo systemctl enable postgresql@9.5-main

```

## Stopping the server safely and quickly

```

sudo systemctl stop SERVICEUNIT

```

```

pg_ctlcluster 9.6 main stop -m fast

```

```

pg_ctl -D datadir -m fast stop

```

## Stopping the server in an emergency

```

pg_ctl -D datadir stop -m immediate

```

```

pg_ctlcluster 9.6 main stop -m immediate

```

## Reloading the server configuration files

```

sudo systemctl reload SERVICEUNIT

```

```

        pg_ctlcluster 9.6 main reload

```

```

        service postgresql reload

```

```

        pg_ctl -D /var/lib/pgsql/data reload

```

```

postgres=# select pg_reload_conf();

```

```

 pg_reload_conf
----------------
 t

```

```

postgres=#  SELECT name, setting, unit
                         ,(source = 'default') as is_default
            FROM pg_settings
            WHERE context = 'sighup'
            AND (name like '%delay' or name like '%timeout')
            AND setting != '0';
            name              | setting | unit | is_default
------------------------------+---------+------+------------
 authentication_timeout       | 60      | s    | t
 autovacuum_vacuum_cost_delay | 20      | ms   | t
 bgwriter_delay               | 10      | ms   | f
 checkpoint_timeout           | 32      | s    | f
 deadlock_timeout             | 1000    | ms   | t
 max_standby_archive_delay    | 30000   | ms   | t
 max_standby_streaming_delay  | 30000   | ms   | t     
 wal_receiver_timeout         | 60000   | ms   | t
 wal_sender_timeout           | 60000   | ms   | t
 wal_writer_delay             | 200     | ms   | t
(9 rows)

```

```

kill -SIGHUP   pid

```

```

kill -SIGHUP 
&grave;psql -t -c "select pid from pg_stat_activity limit 1"&grave;

```

## Restarting the server quickly

```

sudo systemctl restart SERVICEUNIT

```

```

pg_ctlcluster 9.6 main restart -m fast

```

```

pg_ctl -D datadir restart -m fast

```

```

psql -c "CHECKPOINT"

```

## Preventing new connections

```

        ALTER DATABASE foo_db CONNECTION LIMIT 0;

```

```

        ALTER USER foo CONNECTION LIMIT 0;

```

```

          # TYPE   DATABASE  USER         CIDR-ADDRESS    METHOD
            local  all       all          reject
            host   all       all          0.0.0.0/0  reject

```

```

        # TYPE   DATABASE  USER         CIDR-ADDRESS    METHOD
          local  all       postgres                   peer
          local  all       all                      reject
          host   all       all          0.0.0.0/0   reject

```

## Restricting users to only one session each

```

postgres=# ALTER ROLE fred CONNECTION LIMIT 1;
ALTER ROLE

```

```

FATAL: too many connections for role "fred".

```

```

postgres=> SELECT rolconnlimit
             FROM pg_roles
             WHERE rolname = 'fred';
 rolconnlimit
--------------
            1
(1 row)
postgres=> SELECT count(*)
             FROM pg_stat_activity
             WHERE usename = 'fred';
 count
-------
     2
(1 row)

```

## Pushing users off the system

```

SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE ...

```

```

postgres=# SELECT count(pg_terminate_backend(pid))
FROM pg_stat_activity
WHERE usename NOT IN
(SELECT usename
 FROM pg_user
WHERE usesuper);
 count
-------
     1

```

## Deciding on a design for multitenancy

## Using multiple schemas

```

        CREATE SCHEMA finance;
        CREATE SCHEMA sales;

```

```

        CREATE TABLE finance.month_end_snapshot (.....)

```

```

        postgres=# select current_schema;

```

```

         current_schema
        ----------------
         public
        (1 row)

```

```

        ALTER ROLE fiona SET search_path = 'finance';
        ALTER ROLE sally SET search_path = 'sales';

```

```

        >REVOKE ALL ON SCHEMA finance FROM public;
        GRANT ALL ON SCHEMA finance TO fiona;
        REVOKE ALL ON SCHEMA sales FROM public;
GRANT ALL ON SCHEMA sales TO sally;

```

```

        REVOKE ALL ON SCHEMA finance FROM public;
        GRANT USAGE ON SCHEMA finance TO fiona;
        GRANT CREATE ON SCHEMA finance TO fiona;
        REVOKE ALL ON SCHEMA sales FROM public;
        GRANT USAGE ON SCHEMA sales TO sally;
        GRANT CREATE ON SCHEMA sales TO sally;

```

```

        GRANT SELECT ON month_end_snapshot TO public;

```

```

        ALTER DEFAULT PRIVILEGES FOR USER fiona IN SCHEMA finance
         GRANT SELECT ON TABLES TO PUBLIC;

```

## Giving users their own private database

```

        postgres=# create user fred;
        CREATE ROLE
        postgres=# create database fred owner = fred;
        CREATE DATABASE

```

```

        postgres=# BEGIN;
        BEGIN
        postgres=# REVOKE connect ON DATABASE  fred FROM public;
        REVOKE
        postgres=# GRANT connect ON DATABASE fred TO fred;
        GRANT
        postgres=# COMMIT;
        COMMIT
        postgres=# create user bob;
        CREATE ROLE

```

```

        os $ psql -U bob fred
        psql: FATAL:  permission denied for database "fred"
        DETAIL:  User does not have CONNECT privilege.

```

## Running multiple servers on one system

```

        sudo -u postgres pg_createcluster 9.6 main2

```

```

        sudo -u postgres pg_ctlcluster 9.6 main2 start

```

```

        psql --port 5433 -h /var/run/postgresql ...

```

```

        psql --cluster 9.6/main2 ...

```

```

        sudo -u postgres initdb -D /var/lib/pgsql/datadir2

```

```

        sudo -u postgres pg_ctl -D /var/lib/pgsql/datadir2 start

```

## Setting up a connection pool

```

        ; 
        ; pgbouncer configuration example 
        ; 
        [databases] 
        postgres = port=5432 dbname=postgres 
        [pgbouncer] 

        listen_addr = 127.0.0.1 
        listen_port = 6432 
        admin_users = postgres 
        ;stats_users = monitoring userid 
        auth_type = any 
        ; put these files somewhere sensible: 
        auth_file = users.txt 
        logfile = pgbouncer.log 
        pidfile = pgbouncer.pid 

        server_reset_query = DISCARD ALL; 
        ; default values 
        pool_mode = session 
        default_pool_size = 20 
        log_pooler_errors = 0

```

```

        "postgres"    ""

```

```

        postgres=> o users.txt
        postgres=> t
        postgres=> SELECT '"'||rolname||'" "'||rolpassword||'"'
         postgres-> FROM pg_authid;
        postgres=> q

```

```

        pgbouncer -d pgbouncer.ini

```

```

         psql -p 6432 -h 127.0.0.1 -U postgres pgbouncer -c "reload"

```

```

psql -p 6432 pgbouncer -c "SHUTDOWN"

```

## Accessing multiple servers using the same host and port

```

        [databases] 
        myfirstdb = port=5432 host=localhost 
        anotherdb = port=5437 host=localhost 
        sparedb = port=5435 host=localhost

```

```

        $ psql -p 6432 -h 127.0.0.1 -U postgres myfirstdb
        psql (9.6.1)
        Type "help" for help.

        myfirstdb=# show port;
        port 
        ------
        5432
        (1 row)

        myfirstdb=# show server_version;
        server_version 
        ----------------
        9.6.1
        (1 row)

```

```

        myfirstdb=# c anotherdb
        psql (9.6.1, server 9.5.5)
        You are now connected to database "anotherdb" as user "postgres".

```

```

        anotherdb=# show port;
         port 
------
     5437
(1 row)

anotherdb=# show server_version;
     server_version 
----------------
     9.5.5
(1 row)

```

```

myfirstdb=# c pgbouncer
psql (9.6.1, server 1.7.2/bouncer)
You are now connected to database "pgbouncer" as user "postgres".
pgbouncer=# show databases;
   name    |   host    | port | database  | force_user | pool_size | reserve_pool
-----------+-----------+------+-----------+------------+-----------+--------------
 anotherdb | localhost | 5437 | anotherdb |            |        20 |            0
 myfirstdb | localhost | 5432 | myfirstdb |            |        20 |            0
 pgbouncer |           | 6432 | pgbouncer | pgbouncer  |         2 |            0
 sparedb   | localhost | 5435 | sparedb   |            |        20 |            0
(4 rows)

```

## Tables and Data

## Introduction

## Choosing good names for database objects

```

{tablename}_{columnname(s)}_{suffix}

```

```

{tablename}_{actionname}_{after|before}_trig

```

```

ALTER INDEX badly_named_index RENAME TO tablename_status_idx;

```

## Handling objects with quoted names

```

CREATE TABLE "MyCust"
AS
SELECT * FROM cust;

```

```

postgres=# SELECT count(*) FROM mycust;
ERROR:  relation "mycust" does not exist
LINE 1: SELECT * FROM mycust;

```

```

postgres=# SELECT count(*) FROM MyCust;
ERROR:  relation "mycust" does not exist
LINE 1: SELECT * FROM mycust;

```

```

postgres=# SELECT count(*) FROM "MyCust";

```

```

 count
-------
     5
(1 row)

```

```

SELECT * FROM mycust;

```

```

SELECT * FROM MYCUST;

```

```

SELECT * FROM MyCust;

```

```

SELECT * FROM "MyCust";

```

```

postgres=# SELECT quote_ident('MyCust');
 quote_ident
-------------
 "MyCust"
(1 row)
postgres=# SELECT quote_ident('mycust');
 quote_ident
-------------
 mycust
(1 row)

```

```

EXECUTE 'CREATE TEMP TABLE ' || quote_ident(tablename) ||
                '(col1             INTEGER);'

```

## Enforcing the same name and definition for columns

```

CREATE SCHEMA s1;
CREATE SCHEMA s2;
CREATE TABLE s1.X
(col1 smalliER
,col2 TEXT);
CREATE TABLE s2.X
(col1 smallint
,col3 NUMERIC);

```

```

SELECT
 table_schema
,table_name
,column_name
,data_type
  ||coalesce(' ' || text(character_maximum_length), '')
  ||coalesce(' ' || text(numeric_precision), '')
  ||coalesce(',' || text(numeric_scale), '')
  as data_type
FROM information_schema.columns
WHERE column_name IN
(SELECT
 column_name
FROM
(SELECT
  column_name
 ,data_type
 ,character_maximum_length
 ,numeric_precision
 ,numeric_scale
 FROM information_schema.columns
 WHERE table_schema NOT IN ('information_schema', 'pg_catalog')
 GROUP BY
  column_name
 ,data_type
 ,character_maximum_length
 ,numeric_precision
 ,numeric_scale
) derived
GROUP BY column_name
HAVING count(*) > 1
)
AND table_schema NOT IN ('information_schema', 'pg_catalog')
ORDER BY column_name
;

```

```

 table_schema | table_name | column_name |   data_type  
--------------+------------+-------------+---------------
 s2           | x          | col1        | integer 32,0
 s1           | x          | col1        | smallint 16,0
(2 rows)

```

```

SELECT
 table_schema
,table_name
,column_name
,data_type
FROM information_schema.columns
WHERE table_name IN
(SELECT
  table_name
 FROM
 (SELECT DISTINCT
   table_name
  ,def
   FROM
   (SELECT
     table_schema
    ,table_name
    ,string_agg(column_name||' '||data_type, ',' ORDER BY column_name) AS def
    FROM information_schema.columns
    WHERE table_schema NOT IN ('information_schema','pg_catalog')
    GROUP BY
     table_schema
    ,table_name     
    ) t
 ) def
 GROUP BY
  table_name
 HAVING
  count(*) > 1
)
ORDER BY
 table_name
,table_schema
,column_name;

```

```

 table_schema | table_name | column_name | data_type
--------------+------------+-------------+-----------
 s1           | x          | col1        | smallint
 s1           | x          | col2        | text
 s2           | x          | col1        | integer
 s2           | x          | col3        | numeric
(4 rows)

```

```

CREATE OR REPLACE FUNCTION diff_table_definition
(t1_schemaname text
,t1_tablename text
,t2_schemaname text
,t2_tablename text)
RETURNS TABLE
(t1_column_name text
,t1_data_type text
,t2_column_name text
,t2_data_type text
)
LANGUAGE SQL
as
$$
SELECT
 t1.column_name
,t1.data_type
,t2.column_name
,t2.data_type
FROM
 (SELECT column_name, data_type
  FROM information_schema.columns
  WHERE table_schema = $1
      AND table_name = $2
 ) t1
 FULL OUTER JOIN
 (SELECT column_name, data_type
  FROM information_schema.columns
  WHERE table_schema = $3
      AND table_name = $4
 ) t2
 ON t1.column_name = t2.column_name
 AND t1.data_type = t2.data_type
WHERE t1.column_name IS NULL OR t2.column_name IS NULL
;
$$;

```

## Identifying and removing duplicates

```

postgres=# SELECT * FROM cust;
 customerid | firstname | lastname | age
------------+-----------+----------+-----
          1 | Philip    | Marlowe  |  38
          2 | Richard   | Hannay   |  42
          3 | Holly     | Martins  |  25
          4 | Harry     | Palmer   |  36
          4 | Mark      | Hall     |  47
(5 rows)

```

```

CREATE UNLOGGED TABLE dup_cust AS
SELECT *
FROM cust
WHERE customerid IN 
 (SELECT customerid
  FROM cust
  GROUP BY customerid
  HAVING count(*) > 1);

```

```

        UPDATE cust
        SET age = 47
        WHERE customerid = 4
        AND lastname = 'Palmer';

```

```

        DELETE FROM cust
        WHERE customerid = 4
        AND lastname = 'Hall';

```

```

postgres=# SELECT * FROM new_cust;
 customerid
------------
          1
          2
          3
          4
          4
(5 rows)

```

```

        BEGIN;

```

```

        LOCK TABLE new_cust IN SHARE ROW EXCLUSIVE MODE;

```

```

        CREATE TEMPORARY TABLE dups_cust AS 
        SELECT customerid, min(ctid) AS min_ctid 
        FROM new_cust 
        GROUP BY customerid 
        HAVING count(*) > 1;

```

```

        DELETE FROM new_cust 
        USING dups_cust 
        WHERE new_cust.customerid = dups_cust.customerid 
        AND new_cust.ctid != dups_cust.min_ctid;

```

```

        COMMIT;

```

```

        VACUUM new_cust;

```

```

SELECT *
FROM mytable
WHERE  (col1, col2, ... ,colN) IN
(SELECT col1, col2, ... ,colN
 FROM mytable
 GROUP BY  col1, col2, ... ,colN
 HAVING count(*) > 1);

```

```

SELECT customerid, customer_name, ..., min(ctid) AS min_ctid 
FROM ... 
GROUP BY customerid, customer_name, ... 
...;

```

```

DELETE FROM new_cust 
... 
WHERE new_cust.customerid = dups_cust.customerid 
AND new_cust.customer_name = dups_cust.customer_name 
AND ... 
AND new_cust.ctid != dups_cust.min_ctid;

```

## Preventing duplicate rows

```

postgres=# SELECT * FROM newcust;
 customerid
------------
          1
          2
          3
          4
(4 rows)

```

```

        ALTER TABLE newcust ADD PRIMARY KEY(customerid);

```

```

        ALTER TABLE newcust ADD UNIQUE(customerid);

```

```

        CREATE UNIQUE INDEX ON newcust (customerid);

```

```

postgres=# SELECT * FROM partial_unique;

```

```

 customerid | status | close_date
------------+--------+------------
          1 | OPEN   |
          2 | OPEN   |
          3 | OPEN   |
          3 | CLOSED | 2010-03-22
(4 rows)

```

```

CREATE UNIQUE INDEX ON partial_unique (customerid)
  WHERE status = 'OPEN';

```

```

postgres=# CREATE TABLE boxes (name text, position box);
CREATE TABLE
postgres=# INSERT INTO boxes VALUES
                         ('First', box '((0,0), (1,1))');
INSERT 0 1
postgres=# INSERT INTO boxes VALUES
                         ('Second', box '((2,0), (2,1))');
INSERT 0 1
postgres=# SELECT * FROM boxes;
  name  |  position  
--------+-------------
 First  | (1,1),(0,0)
 Second | (2,1),(2,0)
(2 rows)

```

```

postgres=# ALTER TABLE boxes ADD EXCLUDE USING gist 
                         (position WITH &&);
NOTICE:  ALTER TABLE / ADD EXCLUDE will create implicit index "boxes_position_excl" for table "boxes"
ALTER TABLE

```

```

ALTER TABLE newcust ADD EXCLUDE (customerid WITH =);

```

## Duplicate indexes

## Uniqueness without indexes

```

CREATE TABLE t(id serial, descr text); 
INSERT INTO t(descr) VALUES ('First value'); 
INSERT INTO t(id,descr) VALUES (1,'Cheating!');

```

## Real-world example - IP address range allocation

```

CREATE TABLE iprange
(iprange_start inet
,iprange_stop inet
,owner text);
INSERT INTO iprange VALUES 
       ('192.168.0.1','192.168.0.16', 'Simon');
INSERT INTO iprange VALUES 
       ('192.168.0.17','192.168.0.24', 'Gianni');
INSERT INTO iprange VALUES 
       ('192.168.0.32','192.168.0.64', 'Gabriele');

```

```

CREATE TYPE inetrange AS RANGE (SUBTYPE = inet);

```

```

CREATE TABLE iprange2
(iprange inetrange
,owner text);

```

```

INSERT INTO iprange2
VALUES ('[192.168.0.1,192.168.0.16]', 'Simon'); 
INSERT INTO iprange2
VALUES ('[192.168.0.17,192.168.0.24]', 'Gianni'); 
INSERT INTO iprange2
VALUES ('[192.168.0.32,192.168.0.64]', 'Gabriele');

```

```

ALTER TABLE iprange2
 ADD EXCLUDE USING GIST (iprange WITH &&);

```

```

INSERT INTO iprange2
VALUES ('[192.168.0.10,192.168.0.20]', 'Somebody else');
ERROR:  conflicting key value violates exclusion constraint "iprange2_iprange_excl"
DETAIL:  Key (iprange)=([192.168.0.10,192.168.0.20]) conflicts with existing key (iprange)=([192.168.0.1,192.168.0.16]).

```

## Real-world example - range of time

## Real-world example - prefix ranges

## Finding a unique key for a set of data

```

postgres=# select * from ord;

```

```

 orderid | customerid |  amt  
---------+------------+--------
   10677 |          2 |   5.50
    5019 |          3 | 277.44
    9748 |          3 |  77.17
(3 rows)

```

```

postgres=# analyze ord;
ANALYZE

```

```

postgres=# SELECT attname, n_distinct 
                          FROM pg_stats
                          WHERE schemaname = 'public' 
                          AND tablename = 'ord';
  attname   | n_distinct
------------+------------
 orderid    |         -1
 customerid |  -0.666667
 amt        |         -1
(3 rows)

```

```

postgres=# SELECT num_of_values, count(*)
            FROM (SELECT customerid, count(*) AS num_of_values
                         FROM ord
                         GROUP BY customerid) s
            GROUP BY num_of_values
            ORDER BY count(*);
 num_of_values | count
---------------+-------
             2 |     1
             1 |     1
(2 rows)

```

```

SELECT num_of_values, count(*)
FROM (SELECT   customerid, orderid, &mldr;
              ,count(*) AS num_of_values
               FROM ord
               GROUP BY customerid, orderid, &mldr;
               ) s
GROUP BY num_of_values
ORDER BY count(*);

```

## Generating test data

```

        postgres=# SELECT * FROM generate_series(1,5);
         generate_series
        -----------------
                       1
                       2
                       3
                       4
                       5
        (5 rows)

```

```

        postgres=# SELECT date(t) FROM generate_series(now(), now() + '1 week', '1 day') AS f(t);
            date   
        ------------
         2010-03-30
         2010-03-31
         2010-04-01
         2010-04-02
         2010-04-03
         2010-04-04
         2010-04-05
         2010-04-06
        (8 rows)

```

```

            (random()*(2*10^9))::integer

```

```

            (random()*(9*10^18))::bigint

```

```

          (random()*100.)::numeric(5,2)

```

```

          repeat('1',(random()*40)::integer)

```

```

          substr('abcdefghijklmnopqrstuvwxyz',1,   (random()*25)::integer)

```

```

          (ARRAY['one','two','three'])[0.5+random()*3]

```

```

        postgres=# SELECT key
                               ,(random()*100.)::numeric(4,2)
                               ,repeat('1',(random()*25)::integer)
        FROM generate_series(1,10) AS f(key);
         key | numeric |         repeat        
        -----+---------+------------------------
           1 |   83.05 | 1111
           2 |    5.28 | 11111111111111
           3 |   41.85 | 1111111111111111111111
           4 |   41.70 | 11111111111111111
           5 |   53.31 | 1
           6 |   10.09 | 1111111111111111
           7 |   68.08 | 111
           8 |   19.42 | 1111111111111111
           9 |   87.03 | 11111111111111111111
          10 |   70.64 | 111111111111111

        (10 rows)

```

```

        postgres=# SELECT key
                                 ,(random()*100.)::numeric(4,2)
                                 ,repeat('1',(random()*25)::integer)
                                 FROM generate_series(1,10) AS f(key)
                                 ORDER BY random() * 1.0;
         key | numeric |         repeat         
        -----+---------+-------------------------
           4 |   86.09 | 1111
          10 |   28.30 | 11111111
           2 |   64.09 | 111111
           8 |   91.59 | 111111111111111
           5 |   64.05 | 11111111
           3 |   75.22 | 11111111111111111
           6 |   39.02 | 1111
           7 |   20.43 | 1111111
           1 |   42.91 | 11111111111111111111
           9 |   88.64 | 1111111111111111111111

        (10 rows)

```

## Randomly sampling data

```

        postgres=# SELECT count(*) FROM mybigtable;
         count
        -------
         10000
        (1 row)
        postgres=# SELECT count(*) FROM mybigtable 
                                 TABLESAMPLE BERNOULLI(1);
         count
        -------
           106
        (1 row)
        postgres=# SELECT count(*) FROM mybigtable 
                                 TABLESAMPLE BERNOULLI(1);
         count
        -------
           99
        (1 row)

```

```

        postgres=# SELECT count(*) FROM mybigtable; 
         count 
        ------- 
         10000 
        (1 row) 
        postgres=# SELECT count(*) FROM mybigtable 
                                 WHERE random() < 0.01; 
         count 
        ------- 
            95 
        (1 row) 
        postgres=# SELECT count(*) FROM mybigtable 
                                 WHERE random() < 0.01; 
         count 
        ------- 
           106 
        (1 row)

```

```

        pg_dump --exclude-table=MyBigTable > db.dmp 
        pg_dump --table=MyBigTable -schema-only >         mybigtable.schema 
psql -c 'copy (SELECT * FROM MyBigTable
               TABLESAMPLE BERNOULLI (1)) to mybigtable.dat'

```

```

        psql -f db.dmp 
        psql -f mybigtable.schema 
        psql -c 'copy mybigtable from mybigtable.dat'

```

## Loading data from a spreadsheet

```

        "Key","Value" 
        1,"c" 
        2,"d"

```

```

        postgres=# COPY sample FROM sample.csv CSV HEADER 
        postgres=# SELECT * FROM sample; 
         key | value 
        -----+------- 
           1 | c 
           2 | d

```

```

        psql -c 'COPY sample FROM sample.csv CSV HEADER'

```

```

        COPY sample FROM '/mydatafiledirectory/sample.csv' CSV HEADER;

```

## Loading data from flat files

```

LOAD CSV   
   FROM 'GeoLiteCity-Blocks.csv' WITH ENCODING iso-646-us   
        HAVING FIELDS   
        (   
           startIpNum, endIpNum, locId   
        )   
   INTO postgresql://user@localhost:54393/dbname?geolite.blocks   
        TARGET COLUMNS   
        (   
           iprange ip4r using (ip-range startIpNum endIpNum),   
           locId   
        )   
   WITH truncate,   
        skip header = 2,   
        fields optionally enclosed by '"',   
        fields escaped by backslash-quote,   
        fields terminated by 't'   

    SET work_mem to '32 MB', maintenance_work_mem to '64 MB';

```

```

pgloader --summary summary.log example.load

```

## Security

## Introduction

## Typical user role

## The PostgreSQL superuser

```

CREATE USER username SUPERUSER;

```

```

ALTER USER username NOSUPERUSER;

```

```

ALTER USER username SUPERUSER;

```

## Other superuser-like attributes

## Attributes are never inherited

## Revoking user access to a table

```

REVOKE ALL ON table1 FROM user2;

```

```

                               Access privileges
 Schema |  Name  | Type  |     Access privileges     | ...
--------+--------+-------+---------------------------+ ...
 public | table1 | table | postgres=arwdDxt/postgres+| ...
        |        |       | role3=r/postgres         +| ...
        |        |       | role5=a/postgres          | ...
(1 row)

```

```

           List of roles
 Role name | Attributes |   Member of  
-----------+------------+---------------
 user2     |            | {role3, role4}

```

```

REVOKE role3 FROM user2;

```

```

            List of roles
 Role name |  Attributes  | Member of
-----------+--------------+-----------
 role4     | Cannot login | {role5}

```

```

REVOKE role4 FROM user2;

```

## Database creation scripts

```

CREATE TABLE table1( 
... 
); 
GRANT SELECT ON table1 TO webreaders; 
GRANT SELECT, INSERT, UPDATE, DELETE ON table1 TO editors; 
GRANT ALL ON table1 TO admins;

```

## Default search path

```

pguser=# show search_path ;
  search_path  
----------------
 “$user”,public
(1 row)

```

```

pguser=# d x
     Table “public.x”
 Column | Type | Modifiers
--------+------+-----------

```

## Securing views

```

CREATE VIEW for_the_public AS 
  SELECT * FROM reserved_data WHERE importance < 10; 
GRANT SELECT ON for_the_public TO PUBLIC;

```

```

CREATE FUNCTION f(text) 
RETURNS boolean 
COST 0.00000001 
LANGUAGE plpgsql AS $$ 
BEGIN 
  RAISE INFO '$1: %', $1; 
  RETURN true; 
END; 
$$;

```

```

SELECT * FROM for_the_public x WHERE f(x :: text);

```

```

ALTER VIEW for_the_public SET (security_barrier = on);

```

## Granting user access to a table

```

GRANT USAGE ON someschema TO somerole; 
GRANT SELECT, INSERT, UPDATE, DELETE ON someschema.sometable TO somerole; 
GRANT somerole TO someuser, otheruser;

```

## Access to the schema

## Granting access to a table through a group role

```

CREATE GROUP webreaders; 
GRANT SELECT ON pages TO webreaders; 
GRANT INSERT ON viewlog TO webreaders; 
GRANT webreaders TO tim, bob;

```

```

GRANT INSERT, UPDATE, DELETE ON comments TO webreaders;

```

## Granting access to all objects in a schema

```

GRANT SELECT ON ALL TABLES IN SCHEMA staging TO bob;

```

## Granting user access to specific columns

```

CREATE TABLE someschema.sometable2(col1 int, col2 text);

```

```

GRANT SELECT, INSERT ON someschema.sometable2 TO somerole; 
GRANT UPDATE (col2) ON someschema.sometable2 TO somerole;

```

```

SET ROLE TO somerole; 
INSERT INTO someschema.sometable2 VALUES (1, 'One'); 
SELECT * FROM someschema.sometable2 WHERE col1 = 1;

```

```

UPDATE someschema.sometable2 SET col2 = 'The number one'; 

```

```

UPDATE 1

```

```

UPDATE someschema.sometable2 SET col1 = 2;

```

```

ERROR:  permission denied for relation sometable2

```

```

SELECT * FROM t;

```

```

GRANT SELECT ON TABLE t TO u;

```

```

GRANT SELECT (c1,c2,c3) ON TABLE t TO u;

```

```

        GRANT SELECT ON someschema.sometable2 TO somerole; 
        REVOKE SELECT (col1) ON someschema.sometable2 FROM    
        somerole;

```

## Granting user access to specific rows

```

CREATE TABLE someschema.sometable3(col1 int, col2 text);

```

```

ALTER TABLE someschema.sometable3 ENABLE ROW LEVEL SECURITY;

```

```

GRANT SELECT ON someschema.sometable3 TO somerole;

```

```

SELECT * FROM someschema.sometable3;
 col1 |   col2   
------+-----------
    1 | One
   -1 | Minus one
(2 rows)

```

```

CREATE POLICY example1 ON someschema.sometable3
FOR SELECT
TO somerole
USING (col1 > 0);

```

```

SELECT * FROM someschema.sometable3;
 col1 |   col2   
------+-----------
    1 | One
(1 row)

```

```

CREATE POLICY example2 ON someschema.sometable3
FOR INSERT
TO somerole
WITH CHECK (col1 > 0);

```

```

GRANT INSERT ON someschema.sometable3 TO somerole;
SELECT * FROM someschema.sometable3;
 col1 |   col2   
------+-----------
    1 | One
(1 row)

```

```

INSERT INTO someschema.sometable3 VALUES (2, ‘Two’);
SELECT * FROM someschema.sometable3;
 col1 |   col2   
------+-----------
    1 | One
    2 | Two
(2 rows)

```

## Creating a new user

```

pguser@hvost:~$ createuser bob

```

```

pguser@hvost:~$ createuser --interactive alice
Shall the new role be a superuser? (y/n) n
Shall the new role be allowed to create databases? (y/n) y
Shall the new role be allowed to create more new roles? (y/n) n

```

```

CREATE USER bob;
CREATE USER alice CREATEDB;

```

```

pguser=# du alice

```

```

           List of roles
 Role name | Attributes | Member of
-----------+------------+-----------
 alice     | Create DB  | {}

```

## Temporarily preventing a user from connecting

```

pguser=# alter user bob nologin;
ALTER ROLE

```

```

pguser=# alter user bob login;
ALTER ROLE

```

## Limiting the number of concurrent connections by a user

```

pguser=# alter user bob connection limit 0;
ALTER ROLE

```

```

pguser=# alter user bob connection limit 10;
ALTER ROLE

```

```

pguser=# alter user bob connection limit -1;
ALTER ROLE

```

## Forcing NOLOGIN users to disconnect

```

SELECT pg_terminate_backend(pid)
  FROM pg_stat_activity a
   JOIN pg_roles r ON a.usename = r.rolname AND NOT rolcanlogin;

```

## Removing a user without dropping their data

```

testdb=# drop user bob;
ERROR:  role “bob” cannot be dropped because some objects depend on it
DETAIL:  owner of table bobstable
owner of sequence bobstable_id_seq

```

```

pguser=# alter user bob nologin;
ALTER ROLE

```

```

pguser=# GRANT bob TO bobs_replacement;
GRANT

```

```

REASSIGN OWNED BY bob TO bobs_replacement;

```

## Checking whether all users have a secure password

```

test2=# select usename,passwd from pg_shadow where passwd not like ‘md5%’ or length(passwd) <> 35;
 usename  |    passwd   
----------+--------------
 tim      | weakpassword
 asterisk | md5chicken
(2 rows)

```

```

test2=# select usename,passwd from pg_shadow where passwd like ‘md5%’ and length(passwd) = 35;
 usename  |               passwd               
----------+-------------------------------------
 bob2     | md518cf038878cd04fa207e7f5602013a36
(1 row)

```

## Giving limited superuser powers to specific users

```

ALTER ROLE BOB WITH CREATEDB;

```

```

ALTER ROLE BOB WITH CREATEROLE;

```

```

pguser@hvost:~$ psql -U postgres
test2
...
test2=# create table lines(line text);
CREATE TABLE
test2=# copy lines from ‘/home/bob/names.txt’;
COPY 37
test2=# SET ROLE to bob;
SET
test2=> copy lines from ‘/home/bob/names.txt’;
ERROR:  must be superuser to COPY to or from a file
HINT:  Anyone can COPY to stdout or from stdin. psql’s copy command also works for anyone.

```

```

create or replace function copy_from(tablename text, filepath text) 
returns void 
security definer 
as 
$$ 
 declare 
 begin 
      execute 'copy ' || quote_ident(tablename) 
                 || ' from ' || quote_literal(filepath) ; 
 end; 
$$ language plpgsql;

```

```

revoke all on function copy_from( text,  text) from public; 
grant execute on function copy_from( text,  text) to bob;

```

## Writing a debugging_info function for developers

```

create or replace function debugging_info_on() 
returns void 
security definer 
as 
$$ 
  begin 
   set client_min_messages to 'DEBUG1'; 
   set log_min_messages to 'DEBUG1'; 
   set log_error_verbosity to 'VERBOSE'; 
   set log_min_duration_statement to 0; 
 end; 
$$ language plpgsql; 
revoke all on function debugging_info_on() from public; 
grant execute on function debugging_info_on() to bob;

```

```

create or replace function debugging_info_reset() 
returns void 
security definer 
as 
$$ 
  begin 
   set client_min_messages to DEFAULT; 
   set log_min_messages to DEFAULT; 
   set log_error_verbosity to DEFAULT; 
   set log_min_duration_statement to DEFAULT; 
 end; 
$$ language plpgsql;

```

## Auditing DDL changes

```

log_statement = 'ddl'

```

```

/etc/init.d/postgresql reload

```

```

postgres@hvost:~$ egrep -i “create|alter|drop  ”  /var/log/postgresql/postgresql-9.6-main.log

```

```

log_filename = 'postgresql-%Y-%m-%d_%H%M%S.log' 
log_rotation_age = 1d 
log_rotation_size = 10MB

```

## Was the change committed?

## Who made the change?

## Can I find this information from the database?

## You may still miss some DDL...

## Auditing data changes

## Collecting data changes from the server log

## Collecting changes using triggers

```

psql -f audit.sql

```

```

pgbench -i

```

```

SELECT audit.audit_table('pgbench_accounts');

```

```

pgbench -t 1000

```

```

cookbook=# SELECT count(*) FROM audit.logged_actions;
 count
-------
  1000
(1 row)

```

```

cookbook=# x on

```

```

cookbook=# SELECT * FROM audit.logged_actions LIMIT 1;
-[ RECORD 1 ]-----+----------------------------------------------------------------------------------------------------------------------------------------------
event_id          | 1
schema_name       | public
table_name        | pgbench_accounts
relid             | 246511
session_user_name | gianni
action_tstamp_tx  | 2017-01-18 19:48:05.626299+01
action_tstamp_stm | 2017-01-18 19:48:05.626446+01
action_tstamp_clk | 2017-01-18 19:48:05.628488+01
transaction_id    | 182578
application_name  | pgbench
client_addr       |
client_port       |
client_query      | UPDATE pgbench_accounts SET abalance = abalance + -758 WHERE aid = 86061;
action            | U
row_data          | “aid”=>“86061”, “bid”=>“1”, “filler”=>“                                                                                    “, “abalance”=>“0”
changed_fields    | “abalance”=>“-758”
statement_only    | f

```

## Collecting changes using triggers and saving them in another database

```

        psql -f audit.sql auditdb

```

```

        ALTER TABLE audit.logged_actions
           RENAME TO logged_actions_old;

```

```

        CREATE EXTENSION postgres_fdw;

```

```

        CREATE SERVER auditsrv
          FOREIGN DATA WRAPPER postgres_fdw
          OPTIONS (host 'audit.example.net', dbname 'auditdb');

```

```

        CREATE USER MAPPING
          FOR PUBLIC
          SERVER auditsrv;

```

```

        IMPORT FOREIGN SCHEMA audit
          LIMIT TO (logged_actions)
          FROM SERVER auditsrv
          INTO audit;

```

```

        CREATE TABLE emp (
            empname           text NOT NULL,
            salary            integer
        );

```

```

        CREATE TABLE emp_audit(
            operation         text      NOT NULL,
            stamp             timestamp NOT NULL,
            userid            text      NOT NULL,
            empname           text      NOT NULL,
            salary integer
        );

```

```

        CREATE FUNCTION log_emp_audit(
          operation text, userid text, empname text, salary integer
        ) RETURNS VOID AS
        $$
        INSERT INTO emp_audit VALUES($1, now(), $2, $3, $4)
        $$ LANGUAGE SQL;

```

```

        CREATE FUNCTION log_emp_audit(
          operation text, userid text, empname text, salary integer
        ) RETURNS VOID AS
        $$
        INSERT INTO emp_audit VALUES($1, now(), $2, $3, $4)
        $$ LANGUAGE SQL;

```

```

        CREATE OR REPLACE FUNCTION do_emp_audit() RETURNS TRIGGER AS
        $$
            BEGIN
                IF (TG_OP = ‘DELETE’) THEN
                   PERFORM log_emp_audit(‘DEL’, user, OLD.empname, 
                      OLD.salary);
                ELSEIF (TG_OP = ‘UPDATE’) THEN
                  -- save old and new values
                   PERFORM log_emp_audit(‘OLD’, user, OLD.empname, 
                      OLD.salary);
                   PERFORM log_emp_audit(‘NEW’, user, NEW.empname, 
                      NEW.salary);
                ELSEIF (TG_OP = ‘INSERT’) THEN
                   PERFORM log_emp_audit(‘INS’, user, NEW.empname, 
                       NEW.salary);
                END IF;
                RETURN NULL; -- result is ignored since this is an 
                   AFTER trigger
            END;
        $$ LANGUAGE plpgsql;

```

```

        CREATE TRIGGER emp_remote_audit
        AFTER INSERT OR UPDATE OR DELETE ON emp
            FOR EACH ROW EXECUTE PROCEDURE do_emp_audit();

```

## Always knowing which user is logged in

```

postgres=# select current_user, session_user;
 current_user | session_user
--------------+--------------
 postgres     | postgres
postgres=# set role to bob;
SET
postgres=> select current_user, session_user;
 current_user | session_user
--------------+--------------
 bob          | postgres

```

```

        postgres=# create user alice noinherit;
        CREATE ROLE
        postgres=# create user bob noinherit;
        CREATE ROLE

```

```

        postgres=# create group sales;
        CREATE ROLE
        postgres=# create group marketing;
        CREATE ROLE
        postgres=# grant postgres to marketing;
        GRANT ROLE

```

```

        postgres=# grant sales to alice;
        GRANT ROLE
        postgres=# grant marketing to alice;
        GRANT ROLE
        postgres=# grant sales to bob;
        GRANT ROLE

```

## Not inheriting user attributes

## Integrating with LDAP

```

host    all         all         10.10.0.1/16          ldap  
ldapserver=ldap.our.net ldapprefix=“cn=“ ldapsuffix=“, 
  dc=our,dc=net”

```

## Setting up the client to use LDAP

```

ldap://ldap.mycompany.com/dc=mycompany,dc=com?uniqueMember?one?(cn=mydatabase)

```

## Replacement for the User Name Map feature

## Connecting using SSL

```

host database  user  IP-address  IP-mask  auth-method

```

```

host      all         all         192.168.1.0/24     md5 
hostssl   all         all         0.0.0.0/0          md5

```

## Getting the SSL key and certificate

```

openssl genrsa 2048 > server.key 
openssl req -new -x509 -key server.key -out server.crt

```

## Setting up a client to use SSL

## Checking server authenticity

## Using SSL certificates to authenticate

```

openssl genrsa 2048 > client.key 
openssl req -new -x509 -key server.key -out client.crt

```

```

hostssl  all    all    0.0.0.0/0         md5  clientcert=1

```

```

chmod 0600 ~/.postgresql/postgresql.key

```

```

host     all    all    10.10.10.10/32   md5 
hostssl  all    all    10.10.10.10/32   trust   clientcert=1 
hostssl  all    all    all              md5     clientcert=1

```

## Avoiding duplicate SSL connection attempts

## Using multiple client certificates

## Using the client certificate to select the database user

```

hostssl   all    all    0.0.0.0/0         cert

```

```

hostssl   all    all    0.0.0.0/0         cert    map=x509cnmap

```

## Mapping external usernames to database roles

```

map-name system-username database-username

```

```

salesmap   /^(.*)@sales.comp.com$      1 
salesmap   /^(.*)@sales.comp.com$   sales 
salesmap   manager@sales.comp.com    auditor

```

## Encrypting sensitive data

```

pguser@laptop:~$ gpg --gen-key

```

```

pguser@laptop:~$ gpg -a --export “PostgreSQL User (test key for PG Cookbook) <pguser@somewhere.net>“ > public.key
pguser@laptop:~$ gpg -a --export-secret-keys “PostgreSQL User (test key for PG Cookbook) <pguser@somewhere.net>“ > secret.key

```

```

pguser@laptop:~$ sudo chgrp postgres secret.key
pguser@laptop:~$ chmod 440 secret.key
pguser@laptop:~$ ls -l *.key
-rw-r--r-- 1 pguser pguser   1718 2016-03-26 13:53 public.key
-r--r----- 1 pguser postgres 1818 2016-03-26 13:54 secret.key

```

```

create or replace function get_my_public_key() returns text as $$ 
return open('/home/pguser/public.key').read() 
$$ 
language plpythonu; 
revoke all on function get_my_public_key() from public; 
create or replace function get_my_secret_key() returns text as $$ 
return open('/home/pguser/secret.key').read() 
$$ 
language plpythonu; 
revoke all on function get_my_secret_key() from public;

```

```

create or replace function encrypt_using_my_public_key( 
    cleartext text, 
    ciphertext out bytea 
) 
AS $$ 
DECLARE 
    pubkey_bin bytea; 
BEGIN 
    -- text version of public key needs to be passed through function dearmor() to get to raw key 
    pubkey_bin := dearmor(get_my_public_key()); 

    ciphertext := pgp_pub_encrypt(cleartext, pubkey_bin); 
END; 
$$ language plpgsql security definer; 
revoke all on function encrypt_using_my_public_key(text) from public; 
grant execute on function encrypt_using_my_public_key(text) to bob;

```

```

create or replace function decrypt_using_my_secret_key( 
    ciphertext bytea, 
    cleartext out text 
) 
AS $$ 
DECLARE 
    secret_key_bin bytea; 
BEGIN 
    -- text version of secret key needs to be passed through function dearmor() to get to raw binary key 
    secret_key_bin := dearmor(get_my_secret_key()); 

    cleartext := pgp_pub_decrypt(ciphertext, secret_key_bin); 
END; 
$$ language plpgsql security definer; 
revoke all on function decrypt_using_my_secret_key(bytea) from public; 
grant execute on function decrypt_using_my_secret_key(bytea) to bob;

```

```

test2=# select encrypt_using_my_public_key(‘X marks the spot!’);

```

```

encrypt_using_my_public_key | 301301N003223o2152125203252;020007376-z233211H...

```

```

test2=# select decrypt_using_my_secret_key(encrypt_using_my_public_key(‘X marks the spot!’));
 decrypt_using_my_secret_key
-----------------------------
 X marks the spot!
(1 row)

```

## For really sensitive data

## For really, really, really sensitive data!

## Database Administration

## Introduction

## Writing a script that either succeeds entirely or fails entirely

```

BEGIN;command 1;command 2;command 3;COMMIT;

```

```

bash $ psql -1 -f myscript.sql
bash $ psql --single-transaction -f myscript.sql

```

```

DROP VIEW IF EXISTS cust_view;

```

```

CREATE OR REPLACE VIEW cust_view AS SELECT * FROM cust;

```

```

postgres=# CREATE OR REPLACE VIEW cust_view AS 
SELECT col as title1 FROM cust;
CREATE VIEW
postgres=# CREATE OR REPLACE VIEW cust_view 
AS SELECT col as title2 FROM cust;
ERROR:  cannot change name of view column "title1" to "title2"

```

```

postgres=# BEGIN;
BEGIN
postgres=# CREATE TABLE a(x int);
CREATE TABLE
postgres=# BEGIN;
WARNING:  there is already a transaction in progress
BEGIN
postgres=# CREATE TABLE b(x int);
CREATE TABLE
postgres=# COMMIT;
COMMIT
postgres=# ROLLBACK;
NOTICE:  there is no transaction in progress
ROLLBACK

```

```

(begin transaction T1)
  (statement 1)
  (begin transaction T2)
    (statement 2)
  (commit transaction T2)
  (statement 3)
(commit transaction t1)

```

```

BEGIN;
  – (statement 1)
  SAVEPOINT T2;
    – (statement 2)
  RELEASE SAVEPOINT T2; /* we assume that statement 2 does not fail */
  – (statement 3)
COMMIT;

```

## Writing a psql script that exits on the first error

```

$ $EDITOR test.sql
mistake1;
mistake2;
mistake3;

```

```

$ psql -f test.sql
psql:test.sql:1: ERROR:  syntax error at or near "mistake1"
LINE 1: mistake1;
        ^
psql:test.sql:2: ERROR:  syntax error at or near "mistake2"
LINE 1: mistake2;
        ^
psql:test.sql:3: ERROR:  syntax error at or near "mistake3"
LINE 1: mistake3;
        ^

```

```

$ psql -f test.sql -v ON_ERROR_STOP=on
psql:test.sql:1: ERROR:  syntax error at or near "mistake1"
LINE 1: mistake1;
        ^

```

```

$ $EDITOR test.sql
set ON_ERROR_STOP
mistake1;
mistake2;
mistake3;

```

```

$ psql -f test.sql -v ON_ERROR_STOP

```

```

$ $EDITOR ~/.psqlrc
set ON_ERROR_STOP

```

## Investigating a psql error

```

postgres=# set VERBOSITY terse
postgres=# set CONTEXT never
postgres=# select * from missingtable;
ERROR:  relation "missingtable" does not exist at character 15

```

```

postgres=# set VERBOSITY verbose
postgres=# set CONTEXT errors
postgres=# select * from missingtable;
ERROR:  42P01: relation "missingtable" does not exist
LINE 1: select * from missingtable;
                      ^
LOCATION:  parserOpenTable, parse_relation.c:1159

```

## How to do it

```

postgres=# create table wrongname(); 
ERROR:  relation "wrongname" already exists

```

```

postgres=# errverbose  
ERROR:  42P07: relation "wrongname" already exists 
LOCATION:  heap_create_with_catalog, heap.c:1067

```

## Performing actions on many tables

```

postgres=# create schema test;
CREATE SCHEMA
postgres=# create table test.a (col1 INTEGER);
CREATE TABLE
postgres=# create table test.b (col1 INTEGER);
CREATE TABLE
postgres=# create table test.c (col1 INTEGER);
CREATE TABLE

```

```

ALTER TABLE X 
ADD COLUMN last_update_timestamp TIMESTAMP WITH TIME ZONE;

```

```

        postgres=# SELECT relname
              FROM pg_class c
              JOIN pg_namespace n
              ON c.relnamespace = n.oid
              WHERE n.nspname = 'test';

```

```

         relname
        ---------
         a
         b
         c
        (3 rows)

```

```

        postgres=# t on
        postgres=# o multi.sql
        postgres=# SELECT 'ALTER TABLE '|| n.nspname
              || '.' || c.relname ||
        ' ADD COLUMN last_update_timestamp TIMESTAMP WITH TIME ZONE;'
        FROM pg_class c
        JOIN pg_namespace n
        ON c.relnamespace = n.oid
        WHERE n.nspname = 'test';o

```

```

        postgres=# ! cat multi.sql
         ALTER TABLE test.a ADD COLUMN  
         last_update_timestamp TIMESTAMP WITH TIME ZONE;
         ALTER TABLE test.b ADD COLUMN 
         last_update_timestamp TIMESTAMP WITH TIME ZONE;
         ALTER TABLE test.c ADD COLUMN 
         last_update_timestamp TIMESTAMP WITH TIME ZONE;

```

```

        postgres=# i multi.sql
        ALTER TABLE
        ALTER TABLE
        ALTER TABLE

```

```

DO $$
DECLARE t record;
BEGIN
    FOR t IN SELECT c.*, n.nspname
           FROM pg_class c JOIN pg_namespace n
           ON c.relnamespace = n.oid
           WHERE n.nspname = 'test' /* ; not needed */
    LOOP
           EXECUTE 'ALTER TABLE '|| quote_ident(t.nspname) ||
            '.' || quote_ident(t.relname) ||
             ' ADD COLUMN last_update_timestamp ' ||
            'TIMESTAMP WITH TIME ZONE';
    END LOOP;
END $$;

```

```

t on 
o script-:i.sql 
SELECT sql FROM ( 
SELECT 'ALTER TABLE '|| n.nspname || '.' || c.relname || 
' ADD COLUMN last_update_timestamp TIMESTAMP WITH TIME ZONE   DEFAULT now();' as sql 
,row_number() OVER (ORDER BY pg_relation_size(c.oid)) 
FROM pg_class c 
 JOIN pg_namespace n 
 ON c.relnamespace = n.oid 
WHERE n.nspname = 'test' 
ORDER BY 2 DESC) as s  
WHERE row_number % 2 = :i; 
o

```

```

$ psql -v i=0 -f make-script.sql
$ psql -v i=1 -f make-script.sql

```

```

$ psql -f script-0.sql &
$ psql -f script-1.sql &

```

```

WHERE row_number % N = i;

```

## Adding/removing columns on a table

```

ALTER TABLE mytable
ADD COLUMN last_update_timestamp TIMESTAMP WITHOUT TIME ZONE;

```

```

ALTER TABLE mytable
DROP COLUMN last_update_timestamp;

```

```

ALTER TABLE mytable
DROP COLUMN IF EXISTS last_update_timestamp,ADD COLUMN last_update_timestamp TIMESTAMP WITHOUT TIME ZONE;

```

```

UPDATE mytable SET last_update_timestamp = NULL;

```

```

ALTER TABLE mytable 
ADD COLUMN last_update_userid INTEGER DEFAULT 0,ADD COLUMN last_update_comment TEXT;

```

```

ALTER TABLE x 
DROP COLUMN  last_update_timestamp
CASCADE;

```

## Changing the data type of a column

```

postgres=# select * from birthday;

```

```

 name  |  dob  
-------+--------
 simon | 690926
(1 row)

```

```

CREATE TABLE birthday
( name       TEXT, dob        INTEGER);

```

```

postgres=# ALTER TABLE birthday
postgres-# ALTER COLUMN dob SET DATA TYPE text;
ALTER TABLE

```

```

postgres=# ALTER TABLE birthday
postgres-# ALTER COLUMN dob SET DATA TYPE integer;
ERROR:  column "dob" cannot be cast to type integer

```

```

postgres=# ALTER TABLE birthday
               ALTER COLUMN dob SET DATA TYPE integer
               USING dob::integer;
ALTER TABLE

```

```

postgres=# ALTER TABLE birthday
ALTER COLUMN dob SET DATA TYPE date
USING date(to_date(dob::text, 'YYMMDD') -
      (CASE WHEN dob/10000 BETWEEN 16 AND 69 THEN interval '100 
        years'
       ELSE interval '0' END));

```

```

postgres=# select * from birthday;
 name  |    dob
-------+------------
 simon | 26/09/1969
(1 row)

```

```

ALTER TABLE foo
ALTER COLUMN col DROP DEFAULT;
ALTER TABLE foo
ALTER COLUMN col SET DEFAULT 'expression';
ALTER TABLE foo
ALTER COLUMN col SET NOT NULL;
ALTER TABLE foo
ALTER COLUMN col DROP NOT NULL;

```

```

postgres=# ALTER TABLE birthday
ALTER COLUMN dob SET DATA TYPE date
USING date(to_date(dob::text, 'YYMMDD') -
      (CASE WHEN dob/10000 BETWEEN 16 AND 69
      THEN interval '100 years'
      ELSE interval '0' END));

```

```

postgres=# select * from cust;
 customerid | firstname | lastname | age 
------------+-----------+----------+-----
          1 | Philip    | Marlowe  |  38
          2 | Richard   | Hannay   |  42
          3 | Holly     | Martins  |  25
          4 | Harry     | Palmer   |  36
(4 rows)

```

```

postgres=# select * from cust;
 customerid |    custname    | age 
------------+----------------+-----
          1 | Philip Marlowe |  38
          2 | Richard Hannay |  42
          3 | Holly Martins  |  25
          4 | Harry Palmer   |  36
(4 rows)

```

```

ALTER TABLE cust ADD COLUMN custname text NOT NULL DEFAULT '';
UPDATE cust SET custname = firstname || ' ' || lastname;
ALTER TABLE cust DROP COLUMN firstname;
ALTER TABLE cust DROP COLUMN lastname;

```

```

BEGIN;
ALTER TABLE cust
  ALTER COLUMN firstname SET DATA TYPE text
        USING firstname || ' ' || lastname,
  ALTER COLUMN firstname SET NOT NULL,
  ALTER COLUMN firstname SET DEFAULT '', 
  DROP COLUMN lastname;
ALTER TABLE cust RENAME firstname TO custname;
COMMIT;

```

## Changing the definition of a data type

```

CREATE TYPE satellites_urani AS ENUM ('Titania','Oberon');

```

```

CREATE TYPE node AS 
( node_name text,
  connstr text,
  standbys text[]);

```

```

ALTER TYPE satellites_urani ADD VALUE 'Ariel' BEFORE 'Titania';
ALTER TYPE satellites_iovis ADD VALUE 'Umbriel' AFTER 'Ariel';

```

```

ALTER TYPE node
  DROP ATTRIBUTE standbys,
  ADD ATTRIBUTE async_standbys text[],
  ADD ATTRIBUTE sync_standbys text[];

```

```

UPDATE mytable SET mynode = mynode :: text :: node;

```

## Adding/removing schemas

```

CREATE SCHEMA sharedschema;

```

```

CREATE SCHEMA sharedschema AUTHORIZATION scarlett;

```

```

CREATE SCHEMA AUTHORIZATION scarlett;

```

```

DROP SCHEMA str;

```

```

CREATE SCHEMA IF NOT EXISTS str;

```

```

CREATE TABLE str.tb (x int);

```

```

DROP SCHEMA IF EXISTS newschema;
CREATE SCHEMA newschema;

```

```

DROP SCHEMA IF EXISTS newschema CASCADE;

```

```

CREATE SCHEMA foo
       CREATE TABLE account
       (id         INTEGER NOT NULL PRIMARY KEY
       ,balance     NUMERIC(50,2))
       CREATE VIEW accountsample AS
       SELECT *
       FROM account
       WHERE random() < 0.1;

```

## Using schema-level privileges

```

GRANT SELECT ON ALL TABLES IN SCHEMA sharedschema TO PUBLIC;

```

```

ALTER DEFAULT PRIVILEGES IN SCHEMA sharedschema
GRANT SELECT ON TABLES TO PUBLIC;

```

## Moving objects between schemas

```

ALTER TABLE cust
SET SCHEMA anotherschema;

```

```

ALTER SCHEMA existingschema RENAME TO anotherschema;

```

## Adding/removing tablespaces

```

CREATE TABLESPACE new_tablespace
LOCATION '/usr/local/pgsql/new_tablespace';

```

```

DROP TABLESPACE new_tablespace;

```

```

SELECT spcname
      ,relname
      ,CASE WHEN relpersistence = 't' THEN 'temp ' ELSE '' END ||
       CASE 
            WHEN relkind = 'r' THEN  'table' 
            WHEN relkind = 'f' THEN  'foreign table' 
            WHEN relkind = 't' THEN  'TOAST table' 
            WHEN relkind = 'v' THEN  'view' 
            WHEN relkind = 'm' THEN  'materialized view' 
            WHEN relkind = 'S' THEN  'sequence' 
            WHEN relkind = 'c' THEN  'type' 
       ELSE 'index' END as objtype
FROM pg_class c join pg_tablespace ts 
ON (CASE WHEN c.reltablespace = 0 THEN 
         (SELECT dattablespace FROM pg_database
            WHERE datname = current_database())
    ELSE c.reltablespace END) = ts.oid
WHERE relname NOT LIKE 'pg_toast%'
AND relnamespace NOT IN (SELECT oid FROM pg_namespace WHERE nspname 
  IN ('pg_catalog', 'information_schema'))
;

```

```

     spcname      |  relname  |  objtype   
------------------+-----------+------------
 new_tablespace   | x         | table
 new_tablespace   | y         | table
 new_tablespace   | z         | temp table
 new_tablespace   | y_val_idx | index

```

```

ERROR:  tablespace "old_tablespace" is not empty

```

```

ALTER TABLESPACE new_tablespace OWNER TO eliza;

```

```

ALTER USER eliza SET default_tablespace = 'new_tablespace';

```

## Putting pg_xlog on a separate device

```

        [postgres@myhost ~]$ pg_ctl stop

```

```

        [postgres@myhost ~]$ mv $PGDATA/pg_xlog /mnt/newdisk/

```

```

        [postgres@myhost ~]$ ln -s /mnt/newdisk/pg_xlog   $PGDATA/pg_xlog

```

```

        [postgres@myhost ~]$ pg_ctl stop

```

```

        [postgres@myhost ~]$ psql -c 'CREATE TABLE pgxlogtest(x int)'

```

## Tablespace-level tuning

```

ALTER TABLESPACE new_tablespace SET 
(seq_page_cost = 0.05, random_page_cost = 0.1);

```

## Moving objects between tablespaces

```

ALTER TABLE mytable SET TABLESPACE new_tablespace;

```

```

ALTER INDEX mytable_val_idx SET TABLESPACE new_tablespace;

```

```

BEGIN;
ALTER TABLE mytable SET TABLESPACE new_tablespace;
ALTER INDEX mytable_val1_idx SET TABLESPACE new_tablespace;
ALTER INDEX mytable_val2_idx SET TABLESPACE new_tablespace;
COMMIT;

```

```

SET default_tablespace = 'new_tablespace';

```

```

ALTER DATABASE mydb SET default_tablespace = 'new_tablespace';

```

```

ALTER DATABASE mydb SET TABLESPACE new_tablespace;

```

```

SELECT  i.relname   as index_name
,       tsi.spcname as index_tbsp
,       t.relname   as table_name
,       tst.spcname as table_tbsp
FROM (
  pg_class t   /* tables */
  JOIN pg_tablespace tst 
    ON t.reltablespace = tst.oid OR
       (t.reltablespace = 0 AND tst.spcname = 'pg_default'))
JOIN pg_index pgi
  ON pgi.indrelid = t.oid 
JOIN (
  pg_class i  /* indexes */
  JOIN pg_tablespace tsi 
    ON i.reltablespace = tsi.oid OR
       (i.reltablespace = 0 AND tsi.spcname = 'pg_default'))
  ON pgi.indexrelid = i.oid
WHERE i.relname NOT LIKE 'pg_toast%'
  AND i.reltablespace != t.reltablespace;

```

```

postgres=# d y
     Table "public.y"
 Column | Type | Modifiers 
--------+------+-----------
 val    | text | 
Indexes:
    "y_val_idx" btree (val), tablespace "new_tablespace"
Tablespace: "new_tablespace2"

```

```

  relname  |      spcname     | relname |    spcname 
-----------+------------------+---------+---------------
 y_val_idx | new_tablespace   | y       | new_tablespace2
(1 row)

```

## Accessing objects in other PostgreSQL databases

```

        postgres=# CREATE FOREIGN DATA WRAPPER postgresql 
         VALIDATOR postgresql_fdw_validator; 
        CREATE FOREIGN DATA WRAPPER 
        postgres=# CREATE SERVER otherdb
         FOREIGN DATA WRAPPER postgresql
         OPTIONS (host 'foo', dbname 'otherdb', port '5432'); 
        CREATE SERVER 
        postgres=# CREATE USER MAPPING FOR PUBLIC 
        SERVER otherdb; 
        CREATE USER MAPPING

```

```

        SELECT dblink_connect('otherdb');

```

```

        dblink_connect
        ----------------
         OK
        (1 row)

```

```

        postgres=# INSERT INTO audit_log VALUES (current_user, now());

```

```

        postgres=# SELECT dblink_exec('INSERT INTO audit_log VALUES' || 
            ' (current_user, now())', true);

```

```

         dblink_exec  
        ------------- 
         INSERT 0 1 
        (1 row)

```

```

        SELECT generate_series(1,3)

```

```

        SELECT *  
        FROM dblink('SELECT generate_series(1,3)')

```

```

        ERROR:  a column definition list is required for functions returning 
        "record" 
        LINE 2: FROM dblink('SELECT generate_series(1,3)'); 
               ^

```

```

        SELECT * 
       FROM dblink('SELECT generate_series(1,3)') 
         AS link(col1 integer);

```

```

        col1  
        ------ 
            1 
            2 
            3 
        (3 rows)

```

```

        SELECT dblink_disconnect();

```

```

         dblink_connect  
        ---------------- 
         OK 
        (1 row)

```

```

        postgres=# CREATE EXTENSION postgres_fdw;

```

```

        CREATE EXTENSION

```

```

        postgres=# dew 
                             List of foreign-data wrappers 
             Name     | Owner  |       Handler        |       Validator         
        --------------+--------+----------------------+------------------------ 
         postgres_fdw | gianni | postgres_fdw_handler | 
        postgres_fdw_validator 
        (1 row)

```

```

        postgres=# CREATE SERVER otherdb 
        FOREIGN DATA WRAPPER postgres_fdw 
        OPTIONS (host 'foo', dbname 'otherdb', port '5432');

```

```

        CREATE SERVER

```

```

        postgres=# CREATE USER MAPPING FOR PUBLIC SERVER otherdb;

```

```

        CREATE USER MAPPING

```

```

          postgres=# CREATE FOREIGN TABLE ft ( 
            num int , 
            word text ) 
          SERVER otherdb 
          OPTIONS ( 
            schema_name 'public' , 
            table_name 't' );

```

```

          CREATE FOREIGN TABLE

```

```

          postgres=# select * from ft;

```

```

           num | word  
          -----+------ 
          (0 rows)

```

```

        postgres=# insert into ft(num,word) values (1,'One'), (2,'Two'),(3,'Three');

```

```

        INSERT 0 3

```

```

        postgres=# select * from ft;

```

```

         num | word   
        -----+------- 
           1 | One 
           2 | Two 
           3 | Three 
        (3 rows)

```

```

postgres=# SELECT dblink_open('example',
      'SELECT generate_series(1,3)', true);
 dblink_open
-------------
 OK
(1 row)
postgres=# SELECT *
      FROM dblink_fetch('example', 10, true)
       AS link (col1 integer);
 col1
------
    1
    2
    3
(3 rows)

```

```

postgres=# SELECT * 
      FROM dblink_fetch('example', 10, true)
      AS link (col1 integer);
 col1 
------
(0 rows)
postgres=# SELECT dblink_close('example');
 dblink_close 
--------------
 OK(1 row)

```

```

CREATE TABLE my_local_copy (LIKE my_foreign_table);

```

```

SELECT * FROM ft WHERE num = 2;

```

```

SELECT * 
 FROM dblink('otherdb', 
      'SELECT * FROM bigtable') AS link ( ... )
 WHERE filtercolumn > 100;

```

```

SELECT * 
FROM dblink('otherdb', 
     'SELECT * FROM bigtable' ||
     ' WHERE filtercolumn > 100') AS link ( ... );

```

```

CREATE FUNCTION my_task(VOID)
RETURNS SETOF text AS $$
      CONNECT 'dbname=myremoteserver';
      SELECT my_task();
$$ LANGUAGE plproxy;

```

```

CREATE FUNCTION get_cust_email(p_username text)
RETURNS SETOF text AS $$
      CONNECT 'dbname=myremoteserver';
      SELECT email FROM users WHERE username = p_username;
$$ LANGUAGE plproxy;

```

```

CREATE FUNCTION get_cust_email(p_username text)
RETURNS SETOF text AS $$
      CLUSTER 'mycluster';
      RUN ON hashtext(p_username);
      SELECT email FROM users WHERE username = p_username;
$$ LANGUAGE plproxy;

```

## Accessing objects in other foreign databases

```

        CREATE EXTENSION IF NOT EXISTS oracle_fdw;

```

```

        CREATE SERVER myserv
        FOREIGN DATA WRAPPER oracle_fdw
        OPTIONS (dbserver '//myhost/MYDB');
        CREATE USER MAPPING FOR myuser
        SERVER myserv;

```

```

        CREATE FOREIGN TABLE mytab(id bigint, descr text)
        SERVER myserv
        OPTIONS (user 'scott', password 'tiger');

```

```

        INSERT INTO mytab VALUES (-1, 'Minus One');

```

```

        SELECT * FROM mytab WHERE id = -1;

```

```

          id |   descr   
         ----+-----------
          -1 | Minus One
         (1 row)

```

## Updatable views

```

postgres=# SELECT * FROM cust;
 customerid | firstname | lastname | age 
------------+-----------+----------+-----
          1 | Philip    | Marlowe  |  38
          2 | Richard   | Hannay   |  42
          3 | Holly     | Martins  |  25
          4 | Harry     | Palmer   |  36
          4 | Mark      | Hall     |  47
(5 rows)

```

```

CREATE VIEW cust_view AS
SELECT customerid
      ,firstname
      ,lastname
      ,age
FROM cust;

```

```

CREATE VIEW cust_avg AS
SELECT avg(age)
FROM cust;
CREATE VIEW cust_above_avg_age AS
SELECT customerid
            ,substr(firstname, 1, 20) as fname
            ,substr(lastname, 1, 20) as lname
            ,age - 
            (SELECT avg(age)::integer
            FROM cust) as years_above_avg
FROM cust
WHERE age > 
     (SELECT avg(age)
      FROM cust);

CREATE VIEW  potential_spammers AS
SELECT customerid
FROM cust
ORDER BY spam_score(firstname, lastname) DESC
LIMIT 100;

```

```

        CREATE VIEW cust_view AS
        SELECT customerid
              ,firstname
              ,lastname
              ,age
        FROM cust;

```

```

        postgres=# INSERT INTO cust_view
        postgres-# VALUES (5, 'simon', 'riggs', 133);
        ERROR:  cannot insert into a view
        HINT:  You need an unconditional ON INSERT DO INSTEAD rule.

```

```

        CREATE RULE cust_view_insert AS
        ON insert TO cust_view
        DO INSTEAD
        INSERT INTO cust
        VALUES (new.customerid, new.firstname, new.lastname, new.age);

```

```

        postgres=# INSERT INTO cust_view
        postgres-# VALUES (5, 'simon', 'riggs', 133);
        INSERT 0 1

```

```

CREATE RULE cust_view_update AS
ON  update TO cust_view
DO INSTEAD
UPDATE cust SET
 firstname = new.firstname
,lastname = new.lastname
,age = new.age
WHERE customerid = old.customerid;
CREATE RULE cust_view_delete AS
ON  delete TO cust_view
DO INSTEAD
DELETE FROM cust
WHERE customerid = old.customerid;

```

```

CREATE VIEW cust_minor AS
SELECT customerid
      ,firstname
      ,lastname
      ,age
FROM cust
WHERE age < 18;

```

```

CREATE RULE cust_minor_update AS
ON  update TO cust_minor
WHERE new.age < 18
DO INSTEAD
UPDATE cust SET
 firstname = new.firstname
,lastname = new.lastname
,age = new.age
WHERE customerid = old.customerid;

```

```

CREATE RULE cust_minor_update_dummy AS
ON  update TO cust_minor
DO INSTEAD NOTHING;
CREATE RULE cust_minor_update_conditional AS
ON  update TO cust_minor
WHERE new.age < 18
DO INSTEAD
UPDATE cust SET firstname = new.firstname
,lastname = new.lastname
,age = new.age
WHERE customerid = old.customerid;

```

```

UPDATE cust_minor SET age = 19 WHERE customerid = 123;

```

```

CREATE VIEW cust_view AS
SELECT customerid
      ,firstname
      ,lastname
      ,age
FROM cust;

```

```

CREATE TABLE cust_view AS SELECT * FROM cust WHERE false;

```

```

postgres # CREATE RULE "_RETURN" AS
                ON SELECT TO cust_view
                DO INSTEAD 
                SELECT * FROM cust;
CREATE RULE
postgres # CREATE TRIGGER cust_view_modify_after_trig
                AFTER INSERT OR UPDATE OR DELETE ON cust_view
                FOR EACH ROW
                EXECUTE PROCEDURE cust_view_modify_trig_proc();
ERROR:  "cust_view" is not a table

```

```

postgres # DROP TABLE cust_view;
ERROR:  "cust_view" is not a table
HINT:  Use DROP VIEW to remove a view
postgres # DROP VIEW cust_view;
DROP VIEW

```

## Using materialized views

```

CREATE TABLE dish
( dish_id SERIAL PRIMARY KEY
, dish_description text
);

CREATE TABLE eater
( eater_id SERIAL
, eating_date date
, dish_id int REFERENCES dish (dish_id)
);
INSERT INTO dish (dish_description)
VALUES ('Lentils'), ('Mango'), ('Plantain'), ('Rice'), ('Tea');

INSERT INTO eater(eating_date, dish_id)
SELECT floor(abs(sin(n)) * 365) :: int + date '2014-01-01'
, ceil(abs(sin(n :: float * n))*5) :: int
FROM generate_series(1,500000) AS rand(n);

```

```

CREATE VIEW v_dish AS
SELECT dish_description, count(*)
FROM dish JOIN eater USING (dish_id)
GROUP BY dish_description
ORDER BY 1;

```

```

SELECT * FROM v_dish;

```

```

dish_description | count  
------------------+--------
 Lentils          |  64236
 Mango            |  66512
 Plantain         |  74058
 Rice             |  90222
 Tea              | 204972
(5 rows)

```

```

CREATE MATERIALIZED VIEW m_dish AS
SELECT dish_description, count(*)
FROM dish JOIN eater USING (dish_id)
GROUP BY dish_description
ORDER BY 1;

```

```

SELECT * FROM m_dish;

```

```

REFRESH MATERIALIZED VIEW m_dish;

```

```

CREATE UNLOGGED TABLE m_dish AS SELECT * FROM v_dish;

```

## Monitoring and Diagnosis

## Introduction

## Providing PostgreSQL information to monitoring tools

## Finding more information about generic monitoring tools

## Real-time viewing using pgAdmin

```

CREATE EXTENSION adminpack;

```

## Checking whether a user is connected

```

SELECT datname FROM pg_stat_activity WHERE usename = 'bob';

```

## What if I want to know whether a computer is connected?

```

SELECT datname, usename, client_addr, client_port,
       application_name FROM pg_stat_activity;

```

## What if I want to repeatedly execute a query in psql?

```

gabriele=> SELECT count(*) FROM pg_stat_activity;
 count
-------
     1
(1 row)

gabriele=> watch 5
Watch every 5s     Tue Aug 27 21:47:24 2013

 count
-------
     1
(1 row)
&mldr; <snip> &mldr;

```

## Checking which queries are running

```

SET track_activities = on

```

```

SELECT datname, usename, state, query
       FROM pg_stat_activity;

```

```

SELECT datname, usename, state, query
       FROM pg_stat_activity WHERE state = 'active';

```

## Catching queries that only run for a few milliseconds

## Watching the longest queries

```

SELECT
    current_timestamp - query_start AS runtime,
    datname, usename, query
FROM pg_stat_activity
WHERE state = 'active'
ORDER BY 1 DESC;

```

```

SELECT
    current_timestamp - query_start AS runtime,
    datname, usename, query
FROM pg_stat_activity
WHERE state = 'active'
      AND current_timestamp - query_start > '1 min'
ORDER BY 1 DESC;

```

## Watching queries from ps

```

update_process_title = on

```

## Checking which queries are active or blocked

```

SELECT datname
, usename
, wait_event_type
, wait_event
, query
FROM pg_stat_activity
WHERE wait_event IS NOT NULL;

```

```

-[ RECORD 1 ]---+-----------------
datname         | postgres
usename         | gianni
wait_event_type | Lock
wait_event      | relation
query           | select * from t;

```

```

SELECT datname, usename, query
FROM pg_stat_activity
WHERE waiting = true;

```

## No need for the = true part

```

SELECT datname, usename, query
       FROM pg_stat_activity
WHERE waiting;

```

## Do we catch all queries waiting on locks?

```

db=# SELECT pg_sleep(10);
<it "stops" for 10 seconds here>
pg_sleep
----------

(1 row)

```

## Knowing who is blocking a query

```

SELECT datname
, usename
, wait_event_type
, wait_event
, pg_blocking_pids(pid) AS blocked_by
, query
FROM pg_stat_activity
WHERE wait_event IS NOT NULL;

```

```

-[ RECORD 1 ]---+-----------------
datname         | postgres
usename         | gianni
wait_event_type | Lock
wait_event      | relation
blocked_by      | {18142}
query           | select * from t;

```

```

SELECT
    w.query AS waiting_query,
    w.pid AS waiting_pid,
    w.usename AS waiting_user,
    l.query AS locking_query,
    l.pid AS locking_pid,
    l.usename AS locking_user,
    t.schemaname || '.' || t.relname AS tablename
 FROM pg_stat_activity w
       JOIN pg_locks l1 ON w.pid = l1.pid AND NOT l1.granted
       JOIN pg_locks l2 ON l1.relation = l2.relation AND l2.granted
       JOIN pg_stat_activity l ON l2.pid = l.pid
       JOIN pg_stat_user_tables t ON l1.relation = t.relid
 WHERE w.waiting;

```

## Killing a specific session

## Try to cancel the query first

## What if the backend won't terminate?

```

kill -9 <backend_pid>

```

## Using statement_timeout to clean up queries that take too long to run

```

hannu=# SET statement_timeout TO '3 s';
SET
hannu=# SELECT pg_sleep(10);
ERROR:  canceling statement due to statement timeout

```

## Killing idle in transaction queries

```

SELECT pg_terminate_backend(pid)
  FROM pg_stat_activity
WHERE state = 'idle in transaction'
   AND current_timestamp - query_start > '10 min';

```

## Killing the backend from the command line

```

kill -SIGINT <backend_pid>

```

## Detecting an in-doubt prepared transaction

```

SELECT t.schemaname || '.' || t.relname AS tablename,
       l.pid, l.granted
       FROM pg_locks l JOIN pg_stat_user_tables t
       ON l.relation = t.relid;

```

```

-----------+-------+---------
    db.x   |       | t
    db.x   | 27289 | f
(2 rows)

```

## Knowing whether anybody is using a specific table

```

CREATE TEMPORARY TABLE tmp_stat_user_tables AS
       SELECT * FROM pg_stat_user_tables;

```

```

SELECT * FROM pg_stat_user_tables n
  JOIN tmp_stat_user_tables t
    ON n.relid=t.relid
   AND (n.seq_scan,n.idx_scan,n.n_tup_ins,n.n_tup_upd,n.n_tup_del)
    <> (t.seq_scan,t.idx_scan,t.n_tup_ins,t.n_tup_upd,t.n_tup_del);

```

## The quick-and-dirty way

```

SELECT pg_stat_reset();

```

```

CREATE TABLE backup_stat_user_tables AS
       SELECT current_timestamp AS snaptime,
              *
 FROM pg_stat_user_tables;

```

## Collecting daily usage statistics

```

INSERT INTO backup_stat_user_tables
       SELECT current_timestamp AS snaptime,
             *
 FROM pg_stat_user_tables;

```

## Knowing when a table was last used

```

CREATE OR REPLACE FUNCTION table_file_access_info(
   IN schemaname text, IN tablename text,
   OUT last_access timestamp with time zone,
   OUT last_change timestamp with time zone
   ) LANGUAGE plpgsql AS $func$
DECLARE
    tabledir text;
    filenode text;
BEGIN
    SELECT regexp_replace(
        current_setting('data_directory') || '/' || pg_relation_filepath(c.oid),
          pg_relation_filenode(c.oid) || '$', ''),
        pg_relation_filenode(c.oid)
      INTO tabledir, filenode
      FROM pg_class c
      JOIN pg_namespace ns
        ON c.relnamespace = ns.oid
       AND c.relname = tablename
       AND ns.nspname = schemaname;
    RAISE NOTICE 'tabledir: % - filenode: %', tabledir, filenode;
    -- find latest access and modification times over all segments
    SELECT max((pg_stat_file(tabledir || filename)).access),
           max((pg_stat_file(tabledir || filename)).modification)
      INTO last_access, last_change
      FROM pg_ls_dir(tabledir) AS filename
      -- only use files matching <basefilename>[.segmentnumber]
     WHERE filename ~ ('^' || filenode || '([.]?[0-9]+)?$');
END;
$func$;

```

## Usage of disk space by temporary data

```

        SELECT current_setting('temp_tablespaces');

```

```

        WITH temporary_tablespaces AS (
          SELECT unnest(string_to_array(
            current_setting('temp_tablespaces'), ',')
          ) AS temp_tablespace
        )  
        SELECT tt.temp_tablespace,
          pg_tablespace_location(t.oid) AS location,

          pg_tablespace_size(t.oid) AS size
        FROM temporary_tablespaces tt
        JOIN pg_tablespace t ON t.spcname = tt.temp_tablespace
          ORDER BY 1;

```

```

temp_tablespace |   location   |  size
-----------------+--------------+---------
 pgtemp1         | /srv/pgtemp1 | 3633152
 pgtemp2         | /srv/pgtemp2 |  376832
(2 rows)

```

```

SELECT current_setting('data_directory') || '/base/pgsql_tmp'

```

```

SELECT datname, temp_files, temp_bytes, stats_reset
  FROM pg_stat_database;

```

## Finding out whether a temporary file is in use any more

## Logging temporary file usage

## Understanding why queries slow down

```

db_01=# analyse;
ANALYZE
Time: 6231.313 ms
db_01=#

```

## Do the queries return significantly more data than they did earlier?

## Do the queries also run slowly when they are run alone?

```

db=# select count(*) from t;
  count 
---------
 1000000
(1 row)
Time: 329.743 ms

```

## Is the second run of the same query also slow?

## Table and index bloat

```

SELECT pg_relation_size(relid) AS tablesize,schemaname,relname,n_live_tup
FROM pg_stat_user_tables
WHERE relname = <tablename>;

```

## Investigating and reporting a bug

## Producing a daily summary of log file errors

```

log_destination = syslog 
syslog_facility = LOCAL0 
syslog_ident = 'postgres' 
log_line_prefix = 'user=%u,db=%d,client=%h ' 
log_temp_files = 0 
log_statement = ddl 
log_min_duration_statement = 1000 
log_min_messages = info 
log_checkpoints = on 
log_lock_waits = on

```

```

#!/bin/bash
outdir=/var/www/reports
begin=$(date +'%Y-%m-%d %H:00:00' -d '-1 day')
end=$(date +'%Y-%m-%d %H:00:00')
outfile="$outdir/daily-$(date +'%H').html"

pgbadger -q -b "$begin" -e "$end" -o "$outfile " 
  /var/log/postgres.log.1 /var/log/postgres.log

```

```

user@dbhost: $ egrep "FATAL|ERROR" /var/log/postgres.log

```

## Analyzing the real-time performance of your queries

```

shared_preload_libraries = 'pg_stat_statements'

```

```

gabriele=# CREATE EXTENSION pg_stat_statements;
CREATE EXTENSION

```

```

SELECT query FROM pg_stat_statements ORDER BY calls DESC;

```

```

SELECT query, total_time/calls AS avg, calls
       FROM pg_stat_statements ORDER BY 2 DESC;

```

```

SELECT * FROM bands WHERE name = 'AC/DC';
SELECT * FROM bands WHERE name = 'Lynyrd Skynyrd';

```

```

gabriele=# SELECT query, calls FROM pg_stat_statements;
                 query                 | calls
---------------------------------------+-------
 SELECT * FROM bands WHERE name = ?;   |     2
 &mldr; <snip> &mldr;

```

## Regular Maintenance

## Introduction

## Controlling automatic database maintenance

```

autovacuum = on 
track_counts = on 

```

```

autovacuum 
autovacuum_work_mem 
autovacuum_max_workers 
autovacuum_naptime 
autovacuum_analyze_scale_factor 
autovacuum_analyze_threshold 
autovacuum_vacuum_cost_delay 
autovacuum_vacuum_cost_limit 
autovacuum_vacuum_scale_factor 
autovacuum_vacuum_threshold 
autovacuum_freeze_max_age 
autovacuum_multixact_freeze_max_age 
log_autovacuum_min_duration 
vacuum_cost_page_dirty 
vacuum_cost_page_hit 
vacuum_cost_page_miss 
vacuum_cost_page_dirty 
vacuum_freeze_min_age 
vacuum_freeze_table_age 
vacuum_multixact_freeze_min_age 
vacuum_multixact_freeze_table_age 

```

```

ALTER TABLE mytable SET (storage_parameter = value);

```

```

autovacuum_enabled 
autovacuum_vacuum_cost_delay 
autovacuum_vacuum_cost_limit 
autovacuum_vacuum_scale_factor 
autovacuum_vacuum_threshold 
autovacuum_freeze_min_age 
autovacuum_freeze_max_age 
autovacuum_freeze_table_age 
autovacuum_multixact_freeze_min_age 
autovacuum_multixact_freeze_max_age 
autovacuum_multixact_freeze_table_age 
autovacuum_analyze_scale_factor 
autovacuum_analyze_threshold 
log_autovacuum_min_duration 

```

```

toast.autovacuum_enabled 
toast.autovacuum_vacuum_cost_delay 
toast.autovacuum_vacuum_cost_limit 
toast.autovacuum_vacuum_scale_factor 
toast.autovacuum_vacuum_threshold 
toast.autovacuum_freeze_min_age 
toast.autovacuum_freeze_max_age 
toast.autovacuum_freeze_table_age 
toast.autovacuum_multixact_freeze_min_age 
toast.autovacuum_multixact_freeze_max_age 
toast.autovacuum_multixact_freeze_table_age 
toast.log_autovacuum_min_duration 

```

```

2010-04-29 01:33:55 BST (13130) LOG:  automatic vacuum of table "postgres.public.pgbench_accounts": index scans: 1 
      pages: 0 removed, 3279 remain 
      tuples: 100000 removed, 100000 remain 
      system usage: CPU 0.19s/0.36u sec elapsed 19.01 sec 
2010-04-29 01:33:59 BST (13130) LOG:  automatic analyze of table "postgres.public.pgbench_accounts" 
      system usage: CPU 0.06s/0.18u sec elapsed 3.66 sec 

```

```

ALTER TABLE big_table SET (autovacuum_enabled = off);

```

```

ALTER TABLE pgbench_accounts
SET ( toast.autovacuum_enabled = off);

```

```

postgres=# SELECT n.nspname, c.relname,
                      pg_catalog.array_to_string(c.reloptions || array(
                      select 'toast.' || 
x from pg_catalog.unnest(tc.reloptions) x),', ')
                      as relopts
FROM pg_catalog.pg_class c
LEFT JOIN
pg_catalog.pg_class tc ON (c.reltoastrelid = tc.oid)
JOIN
pg_namespace n ON c.relnamespace = n.oid
WHERE c.relkind = 'r'
AND nspname NOT IN ('pg_catalog', 'information_schema');

```

```

 nspname |     relname      |   relopts 
---------+------------------+------------------------------
 public  | pgbench_accounts | fillfactor=100,
                               autovacuum_enabled=on,
                               autovacuum_vacuum_cost_delay=20
 public  | pgbench_tellers  | fillfactor=100
 public  | pgbench_branches | fillfactor=100
 public  | pgbench_history  |
 public  | text_archive     | toast.autovacuum_enabled=off

```

```

include 'autovacuum.conf'  

```

```

        autovacuum = on 
        autovacuum_max_workers = 3 
        include 'autovacuum.conf' 

```

```

        autovacuum_analyze_scale_factor = 0.1 
        autovacuum_analyze_threshold = 50 
        autovacuum_vacuum_cost_delay = 30 
        autovacuum_vacuum_cost_limit = -1 
        autovacuum_vacuum_scale_factor = 0.2 
        autovacuum_vacuum_threshold = 50  

```

```

autovacuum_analyze_scale_factor = 0.05 
autovacuum_analyze_threshold = 50 
autovacuum_vacuum_cost_delay = 10 
autovacuum_vacuum_cost_limit = -1 
autovacuum_vacuum_scale_factor = 0.1 
autovacuum_vacuum_threshold = 50 

```

```

$ ln -sf autovacuum.conf.night autovacuum.conf
$ pg_ctl -D datadir reload

```

## Avoiding auto-freezing and page corruptions

```

SET vacuum_freeze_table_age = 0;
VACUUM;

```

```

initdb --data-checksums

```

## Removing issues that cause bloat

```

postgres=# SELECT now() - case when backend_xid is not null then xact_start else query_start end as age,
pid, backend_xid as xid, backend_xmin as xmin, state
FROM pg_stat_activity ORDER BY 1 desc;
age       |  pid  |  xid   | xmin  |       state
-----------------+-------+--------+-------+---------------------
00:06:59.248634 | 37485 |  40970 |       | idle in transaction
00:00:00.036321 | 37504 | 146914 | 40970 | active 00:00:00.022492 | 37506 | 146916 | 40970 | active 00:00:00.015581 | 37505 | 146917 | 40970 | active 00:00:00.006147 | 37503 | 146918 | 40970 | active 00:00:00        | 37475 |        | 40970 | active

```

## Removing old prepared transactions

```

SHOW max_prepared_transactions;

```

```

postgres=# SELECT * FROM pg_prepared_xacts;
-[ RECORD 1 ]------------------------------
transaction | 121083
gid         | prep1
prepared    | 2010-03-28 15:47:57.637868+01 owner       | postgres
database    | postgres

```

```

COMMIT PREPARED 'prep1';

```

```

ROLLBACK PREPARED 'prep1';

```

```

postgres=# SELECT l.locktype, x.database, l.relation, l.page, l.tuple,l.classid, l.objid, l.objsubid, l.mode, x.transaction, x.gid, x.prepared, x.owner
FROM pg_locks l JOIN pg_prepared_xacts x ON l.virtualtransaction = ‘-1/’ || x.transaction::text;

```

```

postgres=# SELECT DISTINCT x.database, l.relation FROM pg_locks l JOIN pg_prepared_xacts x ON l.virtualtransaction = ‘-1/’ || x.transaction::text WHERE l.locktype != ‘transactionid’; database | relation
----------+----------
postgres |    16390
postgres |    16401
(2 rows)

```

```

SELECT * FROM table WHERE xmax = 121083; 

```

## Actions for heavy users of temporary tables

```

postgres=# SELECT relname, pg_relation_size(oid) FROM pg_class
WHERE relkind in ('i','r') AND relnamespace = 'pg_catalog'::regnamespace
ORDER BY 2 DESC;

```

```

             relname             | pg_relation_size
---------------------------------+------------------
 pg_proc                         |           450560
 pg_depend                       |           344064
 pg_attribute                    |           286720
 pg_depend_depender_index        |           204800
 pg_depend_reference_index       |           204800
 pg_proc_proname_args_nsp_index  |           180224
 pg_description                  |           172032
 pg_attribute_relid_attnam_index |           114688
 pg_operator                     |           106496
 pg_statistic                    |           106496
 pg_description_o_c_o_index      |            98304
 pg_attribute_relid_attnum_index |            81920
 pg_proc_oid_index               |            73728
 pg_rewrite                      |            73728
 pg_class                        |            57344
 pg_type                         |            57344
 pg_class_relname_nsp_index      |            40960
...(partial listing)

```

## Identifying and fixing bloated tables and indexes

```

CREATE OR REPLACE VIEW av_needed AS 
SELECT N.nspname, C.relname 
, pg_stat_get_tuples_inserted(C.oid) AS n_tup_ins 
, pg_stat_get_tuples_updated(C.oid) AS n_tup_upd 
, pg_stat_get_tuples_deleted(C.oid) AS n_tup_del 
, CASE WHEN pg_stat_get_tuples_updated(C.oid) > 0 
       THEN pg_stat_get_tuples_hot_updated(C.oid)::real 
          / pg_stat_get_tuples_updated(C.oid) 
       END 
  AS HOT_update_ratio 
, pg_stat_get_live_tuples(C.oid) AS n_live_tup 
, pg_stat_get_dead_tuples(C.oid) AS n_dead_tup 
, C.reltuples AS reltuples 
, round( current_setting('autovacuum_vacuum_threshold')::integer 
       + current_setting('autovacuum_vacuum_scale_factor')::numeric 
       * C.reltuples) 
  AS av_threshold 
, date_trunc('minute', 
    greatest(pg_stat_get_last_vacuum_time(C.oid), 
             pg_stat_get_last_autovacuum_time(C.oid))) 
  AS last_vacuum 
, date_trunc('minute', 
    greatest(pg_stat_get_last_analyze_time(C.oid), 
             pg_stat_get_last_analyze_time(C.oid))) 
  AS last_analyze 
, pg_stat_get_dead_tuples(C.oid) > 
  round( current_setting('autovacuum_vacuum_threshold')::integer 
       + current_setting('autovacuum_vacuum_scale_factor')::numeric 
       * C.reltuples) 
  AS av_needed 
, CASE WHEN reltuples > 0 
       THEN round(100.0 * pg_stat_get_dead_tuples(C.oid) / reltuples) 
       ELSE 0 END 
  AS pct_dead 
FROM pg_class C 
LEFT JOIN pg_namespace N ON (N.oid = C.relnamespace) 
WHERE C.relkind IN ('r', 't') 
  AND N.nspname NOT IN ('pg_catalog', 'information_schema') 
  AND N.nspname NOT LIKE 'pg_toast%' 
ORDER BY av_needed DESC, n_dead_tup DESC; 

```

```

postgres=# x  
postgres=# SELECT * FROM av_needed WHERE nspname = 'public' AND relname = 'pgbench_accounts';

```

```

-[ RECORD 1 ]----+------------------------
nspname          | public
relname          | pgbench_accounts
n_tup_ins        | 100001
n_tup_upd        | 117201
n_tup_del        | 1
hot_update_ratio | 0.123454578032611
n_live_tup       | 100000
n_dead_tup       | 0
reltuples        | 100000
av_threshold     | 20050
last_vacuum      | 2010-04-29 01:33:00+01
last_analyze     | 2010-04-28 15:21:00+01
av_needed        | f
pct_dead         | 0

```

```

SELECT
nspname,relname,
round(100 * pg_relation_size(indexrelid) /
                    pg_relation_size(indrelid)) / 100
                AS index_ratio,     
  pg_size_pretty(pg_relation_size(indexrelid))
                AS index_size,
  pg_size_pretty(pg_relation_size(indrelid))
                AS table_size
FROM pg_index I
LEFT JOIN pg_class C ON (C.oid = I.indexrelid)
LEFT JOIN pg_namespace N ON (N.oid = C.relnamespace)
WHERE
  nspname NOT IN ('pg_catalog', 'information_schema', 'pg_toast') AND
  C.relkind='i' AND 
  pg_relation_size(indrelid) > 0;

```

```

test=> SELECT * FROM pgstattuple('pg_catalog.pg_proc');

```

```

-[ RECORD 1 ]------+-------
table_len          | 458752
tuple_count        | 1470
tuple_len          | 438896
tuple_percent      | 95.67
dead_tuple_count   | 11
dead_tuple_len     | 3157
dead_tuple_percent | 0.69
free_space         | 8932
free_percent       | 1.95

```

```

Postgres=# select * from pgstattuple_approx('pgbench_accounts');
-[ RECORD 1 ]--------+-----------------
table_len            | 268591104 
scanned_percent      | 0 
approx_tuple_count   | 1001738 
approx_tuple_len     | 137442656 
approx_tuple_percent | 51.1717082037088
dead_tuple_count     | 0 
dead_tuple_len       | 0 
dead_tuple_percent   | 0 
approx_free_space    | 131148448 
approx_free_percent  | 48.8282917962912 

```

```

postgres=> SELECT * FROM pgstatindex('pg_cast_oid_index');
-[ RECORD 1 ]------+------
version            | 2
tree_level         | 0
index_size         | 8192
root_block_no      | 1
internal_pages     | 0
leaf_pages         | 1
empty_pages        | 0
deleted_pages      | 0
avg_leaf_density   | 50.27
leaf_fragmentation | 0

```

## Monitoring and tuning vacuum

```

postgres=# SELECT * FROM pg_stat_progress_vacuum WHERE pid = 34399; 

```

```

Pid                | 34399 
datid              | 12515 
datname            | postgres 
relid              | 16422 
phase              | scanning heap 
heap_blks_total    | 32787 
heap_blks_scanned  | 25207 
heap_blks_vacuumed | 0 
index_vacuum_count | 0 
max_dead_tuples    | 9541017 
num_dead_tuples    | 537600 

```

```

Pid                | 34399 
datid              | 12515 
datname            | postgres 
relid              | 16422 
phase              | vacuuming indexes 
heap_blks_total    | 32787 
heap_blks_scanned  | 32787 
heap_blks_vacuumed | 0 
index_vacuum_count | 0 
max_dead_tuples    | 9541017 
num_dead_tuples    | 999966 

```

```

Pid                | 34399 
datid              | 12515 
datname            | postgres 
relid              | 16422 
phase              | vacuuming heap 
heap_blks_total    | 32787 
heap_blks_scanned  | 32787 
heap_blks_vacuumed | 25051 
index_vacuum_count | 1 
max_dead_tuples    | 9541017 
num_dead_tuples    | 999966 

```

```

$ vacuumdb --jobs=4 --all

```

## Maintaining indexes

```

$ reindexdb

```

```

$ reindexdb -a

```

```

        DROP TABLE IF EXISTS test;
        CREATE TABLE test
        (id INTEGER PRIMARY KEY
        ,category TEXT
        , value TEXT);
        CREATE INDEX ON test (category);

```

```

        SELECT oid, relname, relfilenode
        FROM pg_class
        WHERE oid in (SELECT indexrelid
                      FROM pg_index
                      WHERE indrelid = 'test'::regclass);
          oid  |      relname      | relfilenode
        -------+-------------------+-------------
         16639 | test_pkey         |       16639
         16641 | test_category_idx |       16641
        (2 rows)

```

```

CREATE INDEX CONCURRENTLY new_index
 ON test (category);
BEGIN;
DROP INDEX test_category_idx;
ALTER INDEX new_index RENAME TO test_category_idx;
COMMIT;

```

```

SELECT oid, relname, relfilenode
FROM pg_class
WHERE oid in (SELECT indexrelid
                FROM pg_index
                WHERE indrelid = 'test'::regclass);
  oid  |      relname      | relfilenode
-------+-------------------+-------------
 16639 | test_pkey         |       16639
 16642 | test_category_idx |       16642
(2 rows)

```

```

ALTER TABLE ... ADD PRIMARY KEY USING INDEX ...

```

```

        CREATE UNIQUE INDEX new_pkey ON test (id);

```

```

        SELECT oid, relname, relfilenode
        FROM pg_class
        WHERE oid in (SELECT indexrelid
                         FROM pg_index
                         WHERE indrelid = 'test'::regclass);
          oid  |      relname      | relfilenode
        -------+-------------------+-------------
         16639 | test_pkey         |       16639
         16642 | test_category_idx |       16642
         16643 | new_pkey          |       16643
        (3 rows)

```

```

        BEGIN;
        LOCK TABLE test;
        UPDATE pg_class SET relfilenode = 16643 WHERE oid = 16639;
        UPDATE pg_class SET relfilenode = 16639 WHERE oid = 16643;
        DROP INDEX new_pkey;
        COMMIT;

```

```

SELECT oid, relname, relfilenode
FROM pg_class
WHERE oid in (SELECT indexrelid
                 FROM pg_index
                 WHERE indrelid = 'test'::regclass);

```

```

  oid  |      relname      | relfilenode
-------+-------------------+-------------
 16639 | test_pkey         |       16643
 16642 | test_category_idx |       16642

(2 rows)

```

## Adding a constraint without checking existing rows

```

postgres=# CREATE TABLE ft(fk int PRIMARY KEY, fs text);
CREATE TABLE
postgres=# CREATE TABLE pt(pk int, ps text);
CREATE TABLE
postgres=# INSERT INTO ft(fk,fs) VALUES (1,'one'), (2,'two');
INSERT 0 2
postgres=# INSERT INTO pt(pk,ps) VALUES (1,'I'), (2,'II'), (3,'III');
INSERT 0 3

```

```

postgres=# ALTER TABLE pt ADD CONSTRAINT pc FOREIGN KEY (pk) REFERENCES ft(fk);
ERROR: insert or update on table "pt" violates foreign key constraint 
pc"
DETAIL: Key (pk)=(3) is not present in table "ft".

```

```

postgres=# ALTER TABLE pt ADD CONSTRAINT pc FOREIGN KEY (pk) REFERENCES ft(fk) NOT VALID;
ALTER TABLE

```

```

postgres=# d pt
      Table "public.pt"
 Column |  Type   | Modifiers
--------+---------+-----------
 pk     | integer |
 ps     | text    |
Foreign-key constraints:
    "pc" FOREIGN KEY (pk) REFERENCES ft(fk) NOT VALID

```

```

postgres=# ALTER TABLE pt VALIDATE CONSTRAINT pc;
ERROR: insert or update on table "pt" violates foreign key constraint 
pc"
DETAIL: Key (pk)=(3) is not present in table "ft".

```

```

postgres=# DELETE FROM pt WHERE pk = 3;
DELETE 1
postgres=# ALTER TABLE pt VALIDATE CONSTRAINT pc;
ALTER TABLE
postgres=# d pt
      Table "public.pt"
 Column |  Type   | Modifiers
--------+---------+-----------
 pk     | integer |
 ps     | text    |
Foreign-key constraints:
    "pc" FOREIGN KEY (pk) REFERENCES ft(fk)

```

## Finding unused indexes

```

postgres=# SELECT schemaname, relname, indexrelname, idx_scan FROM pg_stat_user_indexes ORDER BY idx_scan;
 schemaname |       indexrelname       | idx_scan
------------+--------------------------+----------
 public     | pgbench_accounts_bid_idx |        0
 public     | pgbench_branches_pkey    |    14575
 public     | pgbench_tellers_pkey     |    15350
 public     | pgbench_accounts_pkey    |   114400
(4 rows)

```

## Carefully removing unwanted indexes

```

SELECT ir.relname AS indexname 
, it.relname AS tablename 
, n.nspname AS schemaname 
FROM pg_index i 
JOIN pg_class ir ON ir.oid = i.indexrelid 
JOIN pg_class it ON it.oid = i.indrelid 
JOIN pg_namespace n ON n.oid = it.relnamespace 
WHERE NOT i.indisvalid; 

```

```

        CREATE OR REPLACE FUNCTION trial_drop_index(iname TEXT)
        RETURNS VOID 
        LANGUAGE SQL AS $$
        UPDATE pg_index
        SET indisvalid = false
        WHERE indexrelid = $1::regclass;
        $$;

```

```

        CREATE OR REPLACE FUNCTION trial_undrop_index(iname TEXT)
        RETURNS VOID
        LANGUAGE SQL AS $$
        UPDATE pg_index
        SET indisvalid = true
        WHERE indexrelid = $1::regclass;
        $$;

```

## Planning maintenance

## Performance and Concurrency

## Introduction

## Finding slow SQL statements

```

 postgres=# x
 postgres=# dx pg_stat_statements

```

```

-[ RECORD 1 ]---------------------------------------------------------- 
Name        | pg_stat_statements 
Version     | 1.4 
Schema      | public 
Description | track execution statistics of all SQL statements executed

```

```

postgres=# CREATE EXTENSION pg_stat_statements; 
postgres=# ALTER SYSTEM
          SET shared_preload_libraries = 'pg_stat_statements';

```

```

postgres=# SELECT calls, total_time, query FROM pg_stat_statements 
         ORDER BY total_time LIMIT 10;

```

```

postgres=# d pg_stat_statements 
          View "public.pg_stat_statements" 
       Column        |       Type       | Modifiers  
---------------------+------------------+----------- 
 userid              | oid              |  
 dbid                | oid              |  
 queryid             | bigint           |  
 query               | text             |  
 calls               | bigint           |  
 total_time          | double precision |  
 min_time            | double precision |  
 max_time            | double precision |  
 mean_time           | double precision |  
 stddev_time         | double precision |  
 rows                | bigint           |  
 shared_blks_hit     | bigint           |  
 shared_blks_read    | bigint           |  
 shared_blks_dirtied | bigint           |  
 shared_blks_written | bigint           |  
 local_blks_hit      | bigint           |  
 local_blks_read     | bigint           |  
 local_blks_dirtied  | bigint           |  
 local_blks_written  | bigint           |  
 temp_blks_read      | bigint           |  
 temp_blks_written   | bigint           |  
 blk_read_time       | double precision |  
 blk_write_time      | double precision |

```

```

postgres=# ALTER SYSTEM
          SET log_min_duration_statement = 10000;

```

## Collecting regular statistics from pg_stat* views

```

postgres=# CREATE EXTENSION pgstatslog;
CREATE EXTENSION

```

```

SELECT collect_deltas();

```

```

*/5 * * * * /usr/bin/psql -c 'SELECT collect_deltas()' mydbname

```

## Another statistics collection package

## Finding out what makes SQL slow

```

postgres=# EXPLAIN (ANALYZE, BUFFERS) ...SQL...

```

```

postgres=# EXPLAIN (ANALYZE, BUFFERS) SELECT count(*) FROM t;
QUERY PLAN
--------------------------------------------------------------
 Aggregate (cost=4427.27..4427.28 rows=1 width=0)  
(actual time=32.953..32.954 rows=1 loops=1)
 Buffers: shared hit=X read=Y
-> Seq Scan on t (cost=0.00..4425.01 rows=901 width=0)  
(actual time=30.350..31.646 rows=901 loops=1)
Buffers: shared hit=X read=Y
Planning time: 0.045 ms
 Execution time: 33.128 ms
(6 rows)

```

```

SELECT * FROM events ORDER BY id DESC LIMIT 3;

```

```

postgres=# CREATE TABLE events(id SERIAL);
CREATE TABLE
postgres=# INSERT INTO events SELECT generate_series(1,1000000);
INSERT 0 1000000
postgres=# EXPLAIN (ANALYZE)
 SELECT * FROM events ORDER BY id DESC LIMIT 3;
QUERY PLAN
--------------------------------------------------------------
 Limit (cost=25500.67..25500.68 rows=3 width=4) 
(actual time=3143.493..3143.502 rows=3 loops=1)
-> Sort (cost=25500.67..27853.87 rows=941280 width=4)
(actual time=3143.488..3143.490 rows=3 loops=1)
Sort Key: id DESC
Sort Method: top-N heapsort Memory: 25kB
-> Seq Scan on events
(cost=0.00..13334.80 rows=941280 width=4)
(actual time=0.105..1534.418 rows=1000000 loops=1)
Planning time: 0.331 ms
 Execution time: 3143.584 ms
(10 rows)
postgres=# CREATE INDEX events_id_ndx ON events(id);
CREATE INDEX
postgres=# EXPLAIN (ANALYZE)
 SELECT * FROM events ORDER BY id DESC LIMIT 3;
QUERY PLAN
----------------------------------------------------------------------
 Limit (cost=0.00..0.08 rows=3 width=4) (actual 
 time=0.295..0.311 rows=3 loops=1)
-> Index Scan Backward using events_id_ndx on events 
 (cost=0.00..27717.34 rows=1000000 width=4) (actual 
 time=0.289..0.295 rows=3 loops=1)
Total runtime: 0.364 ms
(3 rows)

```

## Not enough CPU power or disk I/O capacity for the current load

```

user@host:~$ top

```

## Locking problems

## EXPLAIN options

```

EXPLAIN (ANALYZE, BUFFERS, FORMAT JSON) SELECT count(*) FROM t;

```

## Reducing the number of rows returned

```

SELECT title, ts_rank_cd(body_tsv, query, 20) AS text_rank
FROM articles, plainto_tsquery('spicy potatoes') AS query
WHERE body_tsv @@ query
ORDER BY rank DESC
LIMIT 20
;

```

```

SELECT title, ts_rank_cd(body_tsv, query, 20) AS text_rank
FROM articles, plainto_tsquery('spicy potatoes') AS query
WHERE body_tsv @@ query
ORDER BY rank DESC, articles.id
OFFSET 20 LIMIT 20;

```

```

SELECT * FROM accounts WHERE branch_id = 7;

```

```

SELECT count(*), sum(balance) FROM accounts WHERE branch_id = 7;

```

```

postgres=# SELECT avg(id) FROM events; 
         avg          
--------------------- 
 500000.500 
(1 row) 
postgres=# SELECT  avg(id)  FROM events TABLESAMPLE system(1); 
         avg          
--------------------- 
 507434.635 
(1 row) 
postgres=# EXPLAIN (ANALYZE, BUFFERS) SELECT avg(id) FROM events; 
                                                       QUERY PLAN                                                        
------------------------------------------------------------------------------------------------------------------------ 
 Aggregate  (cost=16925.00..16925.01 rows=1 width=32) (actual time=204.841..204.841 rows=1 loops=1) 
   Buffers: shared hit=96 read=4329 
   ->  Seq Scan on events  (cost=0.00..14425.00 rows=1000000 width=4) (actual time=1.272..105.452 rows=1000000 loops=1) 
         Buffers: shared hit=96 read=4329 
 Planning time: 0.059 ms 
 Execution time: 204.912 ms 
(6 rows) 
postgres=# EXPLAIN (ANALYZE, BUFFERS) 
          SELECT avg(id) FROM events TABLESAMPLE system(1); 
                                                    QUERY PLAN                                                      
------------------------------------------------------------------------------------------------------------------- 
 Aggregate  (cost=301.00..301.01 rows=1 width=32) (actual time=4.627..4.627 rows=1 loops=1) 
   Buffers: shared hit=1 read=46 
   ->  Sample Scan on events  (cost=0.00..276.00 rows=10000 width=4) (actual time=0.074..2.833 rows=10622 loops=1) 
         Sampling: system ('1'::real) 
         Buffers: shared hit=1 read=46 
 Planning time: 0.066 ms 
 Execution time: 4.702 ms 
(7 rows)

```

## Simplifying complex SQL queries

```

SELECT shop.sp_name AS shop_name,
q1_nloc_profit.profit AS q1_profit,
q2_nloc_profit.profit AS q2_profit,
q3_nloc_profit.profit AS q3_profit,
q4_nloc_profit.profit AS q4_profit,
year_nloc_profit.profit AS year_profit
FROM (SELECT * FROM salespoint ORDER BY sp_name) AS shop
LEFT JOIN (
SELECT
spoint_id,
sum(sale_price) - sum(cost) AS profit,
count(*) AS nr_of_sales
FROM sale s
JOIN item_in_wh iw ON s.item_in_wh_id=iw.id
JOIN item i ON iw.item_id = i.id
JOIN salespoint sp ON s.spoint_id = sp.id
JOIN location sploc ON sp.loc_id = sploc.id
JOIN warehouse wh ON iw.whouse_id = wh.id
JOIN location whloc ON wh.loc_id = whloc.id
WHERE sale_time >= '2013-01-01'
AND sale_time < '2013-04-01'
AND sploc.id != whloc.id
GROUP BY 1
) AS q1_nloc_profit
ON shop.id = Q1_NLOC_PROFIT.spoint_id
LEFT JOIN (
< similar subquery for 2nd quarter >
) AS q2_nloc_profit
ON shop.id = q2_nloc_profit.spoint_id
LEFT JOIN (
< similar subquery for 3rd quarter >
) AS q3_nloc_profit
ON shop.id = q3_nloc_profit.spoint_id
LEFT JOIN (
< similar subquery for 4th quarter >
) AS q4_nloc_profit
ON shop.id = q4_nloc_profit.spoint_id
LEFT JOIN (
< similar subquery for full year >
) AS year_nloc_profit
ON shop.id = year_nloc_profit.spoint_id
ORDER BY 1
;

```

```

CREATE VIEW non_local_quarterly_profit_2013 AS
SELECT
spoint_id,
extract('quarter' from sale_time) as sale_quarter,
sum(sale_price) - sum(cost) AS profit,
count(*) AS nr_of_sales
FROM sale s
JOIN item_in_wh iw ON s.item_in_wh_id=iw.id
JOIN item i ON iw.item_id = i.id
JOIN salespoint sp ON s.spoint_id = sp.id
JOIN location sploc ON sp.loc_id = sploc.id
JOIN warehouse wh ON iw.whouse_id = wh.id
JOIN location whloc ON wh.loc_id = whloc.id
WHERE sale_time >= '2013-01-01'
AND sale_time < '2014-01-01'
AND sploc.id != whloc.id
GROUP BY 1,2;
SELECT shop.sp_name AS shop_name,
q1_nloc_profit.profit as q1_profit,
q2_nloc_profit.profit as q2_profit,
q3_nloc_profit.profit as q3_profit,
q4_nloc_profit.profit as q4_profit,
year_nloc_profit.profit as year_profit
FROM (SELECT * FROM salespoint ORDER BY sp_name) AS shop
LEFT JOIN non_local_quarterly_profit_2013 AS q1_nloc_profit
ON shop.id = Q1_NLOC_PROFIT.spoint_id
AND q1_nloc_profit.sale_quarter = 1
LEFT JOIN non_local_quarterly_profit_2013 AS q2_nloc_profit
ON shop.id = Q2_NLOC_PROFIT.spoint_id
AND q2_nloc_profit.sale_quarter = 2
LEFT JOIN non_local_quarterly_profit_2013 AS q3_nloc_profit
ON shop.id = Q3_NLOC_PROFIT.spoint_id
AND q3_nloc_profit.sale_quarter = 3
LEFT JOIN non_local_quarterly_profit_2013 AS q4_nloc_profit
ON shop.id = Q4_NLOC_PROFIT.spoint_id
AND q4_nloc_profit.sale_quarter = 4
LEFT JOIN (
SELECT spoint_id, sum(profit) AS profit
FROM non_local_quarterly_profit_2013 GROUP BY 1
) AS year_nloc_profit
ON shop.id = year_nloc_profit.spoint_id
ORDER BY 1;

```

```

WITH nlqp AS (
SELECT
spoint_id,
extract('quarter' from sale_time) as sale_quarter,
sum(sale_price) - sum(cost) AS profit,
count(*) AS nr_of_sales
FROM sale s
JOIN item_in_wh iw ON s.item_in_wh_id=iw.id
JOIN item i ON iw.item_id = i.id
JOIN salespoint sp ON s.spoint_id = sp.id
JOIN location sploc ON sp.loc_id = sploc.id
JOIN warehouse wh ON iw.whouse_id = wh.id
JOIN location whloc ON wh.loc_id = whloc.id
WHERE sale_time >= '2013-01-01'
AND sale_time < '2014-01-01'
AND sploc.id != whloc.id
GROUP BY 1,2
)
SELECT shop.sp_name AS shop_name,
q1_nloc_profit.profit as q1_profit,
q2_nloc_profit.profit as q2_profit,
q3_nloc_profit.profit as q3_profit,
q4_nloc_profit.profit as q4_profit,
year_nloc_profit.profit as year_profit
FROM (SELECT * FROM salespoint ORDER BY sp_name) AS shop
LEFT JOIN nlqp AS q1_nloc_profit
ON shop.id = Q1_NLOC_PROFIT.spoint_id
AND q1_nloc_profit.sale_quarter = 1
LEFT JOIN nlqp AS q2_nloc_profit
ON shop.id = Q2_NLOC_PROFIT.spoint_id
AND q2_nloc_profit.sale_quarter = 2
LEFT JOIN nlqp AS q3_nloc_profit
ON shop.id = Q3_NLOC_PROFIT.spoint_id
AND q3_nloc_profit.sale_quarter = 3
LEFT JOIN nlqp AS q4_nloc_profit
ON shop.id = Q4_NLOC_PROFIT.spoint_id
AND q4_nloc_profit.sale_quarter = 4
LEFT JOIN (
SELECT spoint_id, sum(profit) AS profit
FROM nlqp GROUP BY 1
) AS year_nloc_profit
ON shop.id = year_nloc_profit.spoint_id
ORDER BY 1;

```

```

BEGIN;
CREATE TEMPORARY TABLE nlqp_temp ON COMMIT DROP
AS
SELECT
spoint_id,
extract('quarter' from sale_time) as sale_quarter,
sum(sale_price) - sum(cost) AS profit,
count(*) AS nr_of_sales
FROM sale s
JOIN item_in_wh iw ON s.item_in_wh_id=iw.id
JOIN item i ON iw.item_id = i.id
JOIN salespoint sp ON s.spoint_id = sp.id
JOIN location sploc ON sp.loc_id = sploc.id
JOIN warehouse wh ON iw.whouse_id = wh.id
JOIN location whloc ON wh.loc_id = whloc.id
WHERE sale_time >= '2013-01-01'
AND sale_time < '2014-01-01'
AND sploc.id != whloc.id
GROUP BY 1,2
;

```

```

SELECT shop.sp_name AS shop_name,
q1_NLP.profit as q1_profit,
q2_NLP.profit as q2_profit,
q3_NLP.profit as q3_profit,
q4_NLP.profit as q4_profit,
year_NLP.profit as year_profit
FROM (SELECT * FROM salespoint ORDER BY sp_name) AS shop
LEFT JOIN nlqp_temp AS q1_NLP
ON shop.id = Q1_NLP.spoint_id AND q1_NLP.sale_quarter = 1
LEFT JOIN nlqp_temp AS q2_NLP
ON shop.id = Q2_NLP.spoint_id AND q2_NLP.sale_quarter = 2
LEFT JOIN nlqp_temp AS q3_NLP
ON shop.id = Q3_NLP.spoint_id AND q3_NLP.sale_quarter = 3
LEFT JOIN nlqp_temp AS q4_NLP
ON shop.id = Q4_NLP.spoint_id AND q4_NLP.sale_quarter = 4
LEFT JOIN (
select spoint_id, sum(profit) AS profit FROM nlqp_temp GROUP BY 1
) AS year_NLP
ON shop.id = year_NLP.spoint_id
ORDER BY 1
;
COMMIT; -- here the temp table goes away

```

## Using materialized views (long-living, temporary tables)

```

CREATE MATERIALIZED VIEW nlqp_temp AS
SELECT spoint_id,
extract('quarter' from sale_time) as sale_quarter,
sum(sale_price) - sum(cost) AS profit,
count(*) AS nr_of_sales
FROM sale s
JOIN item_in_wh iw ON s.item_in_wh_id=iw.id
JOIN item i ON iw.item_id = i.id
JOIN salespoint sp ON s.spoint_id = sp.id
JOIN location sploc ON sp.loc_id = sploc.id
JOIN warehouse wh ON iw.whouse_id = wh.id
JOIN location whloc ON wh.loc_id = whloc.id
WHERE sale_time >= '2013-01-01'
AND sale_time < '2014-01-01'
AND sploc.id != whloc.id
GROUP BY 1,2

```

## Using set-returning functions for some parts of queries

## Speeding up queries without rewriting them

## Increasing work_mem

```

SET work_mem = '1TB';

```

```

RESET work_mem;

```

```

SET work_mem = '128MB';

```

## More ideas with indexes

```

CREATE INDEX t1_a_b_ndx ON t1(a, b);

```

```

CREATE INDEX t1_proc_ndx ON t1(i1)
WHERE needs_processing = TRUE;

```

```

SELECT id, ... WHERE needs_processing AND i1 = 5;

```

```

CLUSTER t1_a_b_ndx ON t1;

```

```

CLUSTER t1;

```

## Using a TABLESAMPLE view

```

CREATE EXTENSION tsm_system_time;
CREATE SCHEMA fast_access_schema;
CREATE VIEW tablename AS
 SELECT * FROM data_schema TABLESAMPLE system_time(5000); --5 secs
SET search_path = 'fast_access_schema, data_schema';

```

## In case of many updates, set fillfactor on the table

```

ALTER TABLE t1 SET (fillfactor = 70);

```

## Rewriting the schema - a more radical approach

## Discovering why a query is not using an index

```

postgres=# ANALYZE;
ANALYZE

```

```

postgres=# EXPLAIN ANALYZE SELECT count(*) FROM itable WHERE id > 500;
QUERY PLAN
---------------------------------------------------------------------
 Aggregate (cost=188.75..188.76 rows=1 width=0)
(actual time=37.958..37.959 rows=1 loops=1)
-> Seq Scan on itable (cost=0.00..165.00 rows=9500 width=0)
(actual time=0.290..18.792 rows=9500 loops=1)
Filter: (id > 500)
Total runtime: 38.027 ms
(4 rows)
postgres=# SET enable_seqscan TO false;
SET
postgres=# EXPLAIN ANALYZE SELECT count(*) FROM itable WHERE id > 500;
QUERY PLAN
---------------------------------------------------------------------
 Aggregate (cost=323.25..323.26 rows=1 width=0)
(actual time=44.467..44.469 rows=1 loops=1)
-> Index Scan using itable_pkey on itable
(cost=0.00..299.50 rows=9500 width=0)
(actual time=0.100..23.240 rows=9500 loops=1)
Index Cond: (id > 500)
Total runtime: 44.556 ms
(4 rows)

```

## Forcing a query to use an index

```

CREATE INDEX ON customer(id)
 WHERE blocked = false AND subscription_status = 'paid';

```

```

random_page_cost = 4; 
seq_page_cost = 1;

```

```

set random_page_cost = 2;

```

## Using parallel query

```

timing
SELECT count(*) FROM accounts;
count
---------
1000000
(1 row)
Time: 261.652 ms
SET max_parallel_workers_per_gather = 8;
SELECT count(*) FROM accounts;
count
---------
1000000
(1 row)
Time: 180.513 ms

```

```

postgres=# EXPLAIN ANALYZE
 SELECT count(*) FROM demo; 
                QUERY PLAN                                                                  
--------------------------------------------------------------------- 
Finalize Aggregate  (cost=78117.63..78117.64 rows=1 width=8) (actual time=203.426..203.426 rows=1 loops=1) 
   ->  Gather  (cost=78117.21..78117.62 rows=4 width=8) (actual time=203.286..203.421 rows=5 loops=1) 
         Workers Planned: 4 
         Workers Launched: 4 
         ->  Partial Aggregate  (cost=77117.21..77117.22 rows=1 width=8) (actual time=194.315..194.315 rows=1 loops=5) 
               ->  Parallel Seq Scan on demo  (cost=0.00..76863.57 rows=101457 width=0) (actual time=115.632..164.688 rows=200200 loops=5) 
 Planning time: 0.076 ms 
 Execution time: 206.197 ms 
(8 rows)

```

## Using optimistic locking

```

BEGIN;
SELECT * FROM accounts WHERE holder_name ='BOB' FOR UPDATE;
<do some calculations here>
UPDATE accounts SET balance = 42.00 WHERE holder_name ='BOB';
COMMIT;

```

```

SELECT A.*, (A.*::text) AS old_acc_info
FROM accounts a WHERE holder_name ='BOB';
<do some calculations here>
UPDATE accounts SET balance = 42.00
WHERE holder_name ='BOB'
AND (A.*::text) = <old_acc_info from select above>;

```

```

UPDATE accounts
 SET balance = balance - i_amount
WHERE username = i_username
AND balance - i_amount > - max_credit
RETURNING balance;

```

```

CREATE OR REPLACE FUNCTION consume_balance (
i_username text, i_amount numeric(10,2), max_credit numeric(10,2),
OUT success boolean, OUT remaining_balance numeric(10,2)) AS
$$
BEGIN
UPDATE accounts SET balance = balance - i_amount
WHERE username = i_username
AND balance - i_amount > - max_credit
RETURNING balance
INTO remaining_balance;
IF NOT FOUND THEN
success := FALSE;
SELECT balance
FROM accounts
WHERE username = i_username
INTO remaining_balance;
ELSE
success := TRUE;
END IF;
END;
$$ LANGUAGE plpgsql;

```

```

SELECT * FROM consume_balance ('bob', 7, 0);

```

## Reporting performance problems

## Backup and Recovery

## Introduction

## Understanding and controlling crash recovery

```

max_wal_size = 20GB 
checkpoint_timeout = 3600

```

## Planning backups

## Hot logical backups of one database

```

pg_dump -F c pgbench > dumpfile

```

```

pg_dump -F c -f dumpfile pgbench

```

```

pg_dump -v

```

```

pg_restore ––schema-only -v dumpfile | head | grep Started
-- Started on 2010-06-03 09:05:46 BST

```

## Hot logical backups of all databases

```

pg_dumpall -g

```

## Backups of database object definitions

```

pg_dumpall --schema-only > myscriptdump.sql

```

```

pg_dumpall --roles-only > myroles.sql

```

```

pg_dumpall --tablespaces-only > mytablespaces.sql

```

```

pg_dumpall --globals-only > myglobals.sql

```

```

pg_restore --schema-only mydumpfile > myscriptdump.sql

```

## Standalone hot physical database backup

```

        cd $PGDATA
        mkdir ../standalone

```

```

        archive_mode = on
        archive_command = 'test ! -f ../standalone/archiving_active ||
                          cp -i %p ../standalone/archive/%f'

```

```

        wal_level = replica

```

```

        cd $PGDATA
        mkdir ../standalone/archive
        touch ../standalone/archiving_active

```

```

        BACKUP_FILENAME=$(date '+%Y%m%d%H%M').tar

```

```

        psql -c "select pg_start_backup('standalone')"

```

```

        tar -cv --exclude="pg_xlog/*"  
        -f ../standalone/$BACKUP_FILENAME *

```

```

        psql -c "select pg_stop_backup(), current_timestamp"

```

```

        cd ../standalone
        rm archiving_active 

```

```

        tar -rf $BACKUP_FILENAME archive  

```

```

        echo "restore_command = 'cp archive/%f %p'" > recovery.conf
        echo "recovery_end_command = 'rm -R archive' " >> recovery.conf

```

```

        tar -rf $BACKUP_FILENAME recovery.conf 

```

```

        rm -rf archive recovery.conf

```

## Hot physical backup and continuous archiving

```

        archive_mode = on 
        archive_command = 'rsync -a %p $DRNODE:/archive/%f'

```

```

        BACKUP_NAME=$(date '+%Y%m%d%H%M')

```

```

        psql -c "select pg_start_backup('$BACKUP_NAME')"

```

```

        rsync -cva --inplace -exclude='pg_xlog/*' 
        ${PGDATA}/  $DRNODE:/backups/base/$BACKUP_NAME/  

```

```

        psql -c "select pg_stop_backup(), current_timestamp"  

```

## Recovery of all databases

## Logical - from custom dump taken with pg_dump -F c

```

        pg_restore --schema-only -v dumpfile | head | grep Started

```

```

        psql -f myglobals.sql

```

```

        pg_restore -C -d postgres -j 4 dumpfile

```

## Logical - from the script dump created by pg_dump -F p

```

        head myscriptdump.sql | grep Started 

```

```

        psql -f myglobals.sql  

```

```

        psql -f myscriptdump.sql  

```

## Logical - from the script dump created by pg_dumpall

```

        head myscriptdump.sql | grep Started 

```

```

        psql -f myscriptdump.sql 

```

## Physical

```

        $ cat backup_label
        START WAL LOCATION: 0/12000020 (file 000000010000000000000012)
        CHECKPOINT LOCATION: 0/12000058
        START TIME: 2010-06-03 19:53:23 BST
        LABEL: standalone

```

```

restore_command = 'scp backup1:/backups/pg/servername/archive/%f 
  %p'

```

## Recovery to a point in time

```

recovery_target_time = '2010-06-01 16:59:14.27452+01'

```

```

SELECT pg_create_restore_point('before_critical_update');

```

## Recovery of a dropped/damaged table

## Logical - from custom dump taken with pg_dump -F c

```

pg_restore -t mydroppedtable dumpfile | psql

```

```

        pg_restore -a -t mydamagedtable dumpfile >     mydamagedtable.sql

```

```

        BEGIN;
        TRUNCATE mydamagedtable;
        i mydamagedtable.sql
        COMMIT;

```

```

        psql -f repair_mydamagedtable.sql

```

```

        CREATE DATABASE restorework;

```

```

        pg_restore -s -d restorework dumpfile

```

```

        pg_dump -t mydroppedtable -s restorework >   mydroppedtable.sql

```

```

        psql -f mydroppedtable.sql

```

```

        pg_restore -t mydroppedtable -a -d maindb dumpfile

```

## Logical - from the script dump

```

        psql -f myscriptdump.sql

```

```

        pg_dump -t mydroppedtable -F c mydatabase > dumpfile

```

```

        pg_restore -d mydatabase -j 2 dumpfile

```

## Physical

```

        pg_dump -t mydroppedtable -F c mydatabase > dumpfile

```

```

        pg_restore -d mydatabase -j 2 dumpfile

```

## Recovery of a dropped/damaged database

## Logical - from the custom dump -F c

```

pg_restore -h myhost -d postgres --create -j 4 dumpfile

```

## Logical - from the script dump created by pg_dump

```

createdb -h myhost myfreshdb
psql -h myhost -f myscriptdump.sql myfreshdb

```

## Logical - from the script dump created by pg_dumpall

```

        psql -f myscriptdump.sql

```

## Physical

## Improving performance of backup/recovery

```

        export PGOPTIONS ="-c work_mem = 128000"

```

```

        pg_restore -j NumJobs

```

## Incremental/differential backup and restore

## Hot physical backups with Barman

```

yum install barman  

```

```

apt-get install barman  

```

```

        compression = gzip

```

```

        [angus] 
        description =  "PostgreSQL Database on angus" 
        active = off 
        archiver = on 
        backup_method = rsync 
        ssh_command = ssh postgres@angus 
        conninfo = host=angus user=barman dbname=postgres

```

```

        [root@malcolm]# barman list-server
        angus - PostgreSQL Database on angus (inactive)

```

```

        [root@malcolm]# barman show-server angus

        Server angus (inactive):
          active: False
          archive_command: None
          archive_mode: None
               &mldr;
               incoming_wals_directory:   /var/lib/barman/angus/incoming
               &mldr;

```

```

        [root@malcolm]#

        barman check angus
        Server angus (inactive):
          WAL archive: FAILED (please make sure WAL shipping is setup)
          PostgreSQL: OK
          superuser: OK
          wal_level: FAILED (please set it to a higher level than 'minimal')
          directories: OK
          retention policy settings: OK
          backup maximum age: OK (no last_backup_maximum_age provided)
          compression settings: OK
          failed backups: OK (there are 0 failed backups)
          minimum redundancy requirements: OK (have 0 backups, expected at least 0)
          ssh: OK (PostgreSQL server)
          not in recovery: OK
          archive_mode: FAILED (please set it to 'on' or 'always')
          archive_command: FAILED (please set it accordingly to documentation)
          archiver errors: OK

        [root@malcolm]# echo $?
        1

```

```

        archive_mode = on
        archive_command = 'rsync -a %p      barman@malcolm:/var/lib/barman/angus/incoming/%f'

        wal_level = replica

```

```

        [root@malcolm]# barman -q check angus 
        [root@malcolm]# echo $?
        0

```

```

        WAL archive: FAILED (please make sure WAL shipping is setup)

```

```

        [root@malcolm]# barman switch-xlog --force --archive angus
        [root@malcolm]# barman archive-wal angus

```

```

        [root@malcolm]# barman backup angus

        Starting backup using rsync-exclusive method for server angus in /var/lib/barman/angus/base/20161003T194717
        Backup start at xlog location: 0/3000028    (000000010000000000000003, 00000028)
        This is the first backup for server angus 
        WAL segments preceding the current backup have been found:
         000000010000000000000001 from server angus has been removed
        Copying files.
        Copy done.
        This is the first backup for server angus
        Asking PostgreSQL server to finalize the backup.
        Backup size: 21.1 MiB
        Backup end at xlog location: 0/3000130   (000000010000000000000003, 00000130)
        Backup completed
        Processing xlog segments from file archival for angus
          000000010000000000000002
          000000010000000000000003
          000000010000000000000003.00000028.backup

```

```

[root@malcolm ~]# cat /etc/cron.d/barman
# m h  dom mon dow   user     command
  * *    *   *   *   barman   [ -x /usr/bin/barman ] && /usr/bin/barman -q cron

```

```

[root@malcolm ~]# barman list-backup angus
angus 20161003T194717 - Mon Oct  3 19:47:20 2016 - Size: 21.1 MiB - WAL Size: 26.6 KiB

```

```

[root@malcolm ~]# barman show-backup angus 20161003T194717 

```

```

Backup 20160930T130002:
  Server Name            : skynyrd
  Status                 : DONE
  PostgreSQL Version     : 90409
  PGDATA directory       : /srv/pgdata

  Base backup information:
    Disk usage           : 16.4 TiB (16.4 TiB with WALs)
    Incremental size     : 5.7 TiB (-65.08%)
    Timeline             : 1
    Begin WAL            : 000000010000358800000063
    End WAL              : 00000001000035A0000000A2
    WAL number           : 6208
    WAL compression ratio: 79.15%
    Begin time           : 2016-09-30 13:00:04.245110+00:00
    End time             : 2016-10-01 13:24:47.322288+00:00
    Begin Offset         : 24272
    End Offset           : 11100576
    Begin XLOG           : 3588/63005ED0
    End XLOG             : 35A0/A2A961A0

  WAL information:
    No of files          : 3240
    Disk usage           : 11.9 GiB
    WAL rate             : 104.33/hour
    Compression ratio    : 76.43%
    Last available       : 00000001000035AD0000004A

  Catalog information:
    Retention Policy     : not enforced
    Previous Backup      : 20160923T130001
    Next Backup          : - (this is the latest base backup)

```

## Recovery with Barman

```

barman show-backup bon last 

```

```

PGDATA directory  : /var/lib/pgsql/9.6/data

```

```

barman recover --remote-ssh-command 'ssh postgres@brian' bon last /var/lib/pgsql/9.6/data 

```

```

Starting remote restore for server bon using backup 20161003T194717
Destination directory: /var/lib/pgsql/9.6/data
Copying the base backup.
Copying required WAL segments.
Generating archive status files
Identify dangerous settings in destination directory.

IMPORTANT
These settings have been modified to prevent data losses

postgresql.conf line 645: archive_command = false

Your PostgreSQL server has been successfully prepared for recovery!

```

```

systemctl start postgresql-9.6 

```

```

systemctl start postgresql 

```

## Replication and Upgrades

## Introduction

## Replication concepts

## Topics

## Basic concepts

## History and scope

## Practical aspects

## Data loss

## Single-master replication

## Multinode architectures

## Clustered or massively parallel databases

## Multimaster replication

## Scalability tools

## Other approaches to replication

## Replication best practices

## Setting up file-based replication - deprecated

```

        wal_level = 'archive' 
        archive_mode = on 
        archive_command = 'scp %p $STANDBYNODE:$PGARCHIVE/%f' 
        archive_timeout = 30

```

```

        psql -c "select pg_start_backup('base backup for log shipping')"

```

```

        rsync -cva --inplace --exclude=*pg_xlog*  
        ${PGDATA}/ $STANDBYNODE:$PGDATA

```

```

    psql -c "select pg_stop_backup(), current_timestamp"

```

```

        standby_mode = 'on' 
        restore_command = 'cp $PGARCHIVE/%f %p' 
        archive_cleanup_command = 'pg_archivecleanup $PGARCHIVE %r' 
        trigger_file = '/tmp/postgresql.trigger.5432'

```

```

archive_command = 'myarchivescript %p %f'

```

```

scp $1 $STANDBYNODE1:$PGARCHIVE/$2 
scp $1 $STANDBYNODE2:$PGARCHIVE/$2 
scp $1 $STANDBYNODE3:$PGARCHIVE/$2

```

```

ps -ef | grep archiver        on master
postgres: archiver process  last was  000000010000000000000040
ps -ef | grep startup        on standby
postgres: startup process  waiting for  000000010000000000000041

```

## Setting up streaming replication

```

        CREATE USER repuser
           REPLICATION
           LOGIN
           CONNECTION LIMIT 2
           ENCRYPTED PASSWORD 'changeme';

```

```

        host   replication   repuser   0.0.0.1/0   md5

```

```

        log_connections = on

```

```

        max_wal_senders = 2 
        wal_level = 'archive' 
        archive_mode = on 
        archive_command = 'cd .'

```

```

        pg_basebackup -d 'connection string' -D /path/to_data_dir

```

```

        --xlog-method=stream

```

```

        --max-rate=RATE

```

```

       --slot=myslotname

```

```

        standby_mode = 'on' 
        primary_conninfo = 'host=192.168.0.1 user=repuser' 
        # trigger_file = ''  # no need for trigger file 9.1+

```

```

        pg_basebackup -F -z

```

```

        psql -c "select pg_start_backup('base backup for streaming rep')"

```

```

        rsync -cva --inplace --exclude=*pg_xlog*  
        ${PGDATA}/ $STANDBYNODE:$PGDATA

```

```

        psql -c "select pg_stop_backup(), current_timestamp"

```

```

        standby_mode = 'on' 
        primary_conninfo = 'host=alpha user=repuser' 
        trigger_file = '/tmp/postgresql.trigger.5432'

```

```

FATAL:  requested WAL segment 000000010000000000000002 has already been removed

```

## Setting up streaming replication security

```

        ALTER ROLE replogin REPLICATION;

```

```

        CREATE ROLE replogin REPLICATION LOGIN;

```

```

       ALTER ROLE replogin CONNECTION LIMIT 2;

```

## Hot Standby and read scalability

```

wal_level = 'hot_standby' # PostgreSQL 9.0 to 9.5
wal_level = ‘replica’ # PostgreSQL 9.6

```

```

hot_standby = on

```

```

SELECT datname, conflicts FROM pg_stat_database;

```

```

    SELECT
  datname
 ,confl_tablespace
 ,confl_lock
 ,confl_snapshot
 ,confl_bufferpin
 ,confl_deadlock
 FROM pg_stat_database_conflicts;

```

## Managing streaming replication

```

       trigger_file = '/tmp/postgresql.trigger.5432'

```

```

touch /tmp/postgresql.trigger.5432

```

## Using repmgr

```

        repmgr master register

```

```

        repmgr standby register

```

```

        repmgr standby clone node1 -D /path/of_new_data_directory

```

```

        repmgr standby clone --force
        repmgr standby promote

```

```

        repmgr standby promote

```

```

        repmgr standby follow

```

```

        repmgr cluster show

```

```

        repmgr cluster cleanup

```

```

        repmgr witness create

```

```

cluster=demo 
node=2 
node_name=beta 
conninfo='host=node2 user=repmgr'

```

```

repmgrd -d -f /var/lib/pgsql/repmgr/repmgr.conf &

```

```

$ psql -x -c "SELECT * FROM repmgr.repl_status"
-[ RECORD 1 ]-------------+------------------------------
primary_node              | 1
standby_node              | 2
last_monitor_time         | 2015-02-23 08:19:39.791974-05
last_wal_primary_location | 0/1902D5E0
last_wal_standby_location | 0/1902D5E0
replication_lag           | 0 bytes
apply_lag                 | 0 bytes
time_lag                  | 00:26:13.30293

```

## Using replication slots

```

        max_replication_slots = 2

```

```

        SELECT (pg_create_physical_replication_slot
        ('alpha_beta_1', true)).xlog_position;
        xlog_position
-----------------
 0/5000060

```

```

        SELECT * FROM pg_replication_slots;

```

```

        primary_slot_name = 'alpha_beta_1'

```

```

        SELECT pg_drop_physical_replication_slot('alpha_beta_1');

```

## Monitoring replication

```

SELECT pg_is_in_recovery();

```

```

SELECT pg_is_xlog_replay_paused();

```

```

SELECT pg_is_in_backup();

```

```

SELECT pg_current_xlog_insert_location();

```

```

SELECT pg_current_xlog_location();

```

```

SELECT pg_last_xlog_receive_location();

```

```

SELECT pg_last_xlog_replay_location();

```

```

SELECT pid, application_name /* or other unique key */
,pg_current_xlog_insert_location() /* WAL Insert location */
,sent_location /* WALSender location */
,write_location /* WALReceiver write loc */
,flush_location /* WALReceiver flush loc */
,replay_location /* Standby apply location */
,backend_start /* Backend start */
FROM pg_stat_replication;
-[ RECORD 1 ]-------------------+------------------------------ 
pid                             | 16496 
application_name                | pg_basebackup 
pg_current_xlog_insert_location | 0/80000D0 
sent_location                   |  
write_location                  |  
flush_location                  |  
replay_location                 |  
backend_start                   | 2017-01-27 15:25:42.988149+00 
-[ RECORD 2 ]-------------------+------------------------------ 
pid                             | 16497 
application_name                | pg_basebackup 
pg_current_xlog_insert_location | 0/80000D0 
sent_location                   | 0/80000D0 
write_location                  | 0/8000000 
flush_location                  | 0/8000000 
replay_location                 |  
backend_start                   | 2017-01-27 15:25:43.18958+00

```

```

SELECT last_msg_receipt_time - last_msg_send_time
FROM pg_stat_wal_receiver;

```

```

SELECT pg_last_xact_replay_timestamp();

```

```

SELECT slot_name, database, age(xmin), age(catalog_xmin)
 FROM pg_replication_slots
 WHERE NOT active;

```

```

SELECT slot_name
 FROM pg_replication_slots
 JOIN pg_stat_replication ON pid = active_pid;

```

## Performance and synchronous replication

```

SET synchronous_commit = on;

```

```

synchronous_standby_names = 'nodeB, nodeC, nodeD'

```

```

synchronous_standby_names = '2 (nodeB, nodeC, nodeD)'

```

```

    SELECT
 application_name
 ,state                    /* startup, backup, catchup or streaming */
 ,sync_priority            /* 0, 1 or more */ 
 ,sync_state               /* async, sync or potential */
 FROM pg_stat_replication
 ORDER BY sync_priority;

```

```

LOG standby $APPLICATION_NAME is now the synchronous 
standby with priority N

```

```

CREATE TABLE TransactionCheck
 (TxnId     SERIAL PRIMARY KEY);

```

```

INSERT INTO TransactionCheck DEFAULT VALUES RETURNING TxnId;

```

## Delaying, pausing, and synchronizing replication

```

SELECT pg_xlog_replay_pause();

```

```

SELECT pg_xlog_replay_resume();

```

```

        SELECT pg_create_restore_point('my action name')

```

```

        SELECT pg_current_xlog_write_location();

```

```

    SELECT pg_last_xlog_replay_location();

```

```

CREATE OR REPLACE FUNCTION wait_for_lsn(loc pg_lsn)
RETURNS VOID
LANGUAGE plpgsql
AS $$
BEGIN

    LOOP
        IF pg_last_xlog_replay_location() IS NULL OR
           pg_last_xlog_replay_location() >= loc THEN
            RETURN;
        END IF;
        PERFORM pg_sleep(0.1);  /* 100ms */
    END LOOP;
END $$;

```

```

        SELECT alter_subscription_disable(); 
        SELECT alter_subscription_enable();

```

## Logical replication

```

        CREATE EXTENSION pglogical;

```

```

        CREATE EXTENSION pglogical_origin;

```

```

        wal_level = 'logical'

```

```

          SELECT pglogical.create_node(
                 node_name := 'nodeA',
                 dsn := 'host=nodeA dbname=postgres');

```

```

# Record data for Logical replication 
wal_level = 'logical' 
# Load the pglogical extension 
shared_preload_libraries = 'pglogical' 
# Allow replication slot creation (we need just one but it 
does not hurt to have more) 
max_replication_slots = 10 
# Allow streaming replication (we need one for slot and 
one for basebackup but again, it does not hurt to have more) 
max_wal_senders = 10 
max_worker_processes = 10

```

```

ALTER TABLE mytable REPLICA IDENTITY USING INDEX myuniquecol_idx;

```

```

        SELECT pglogical.replication_set_add_all_tables(
                 set_name := 'default',
                 schema_names := ARRAY['public'],
                 true);

```

```

        SELECT pglogical.create_subscription(
                 subscription_name := 'my_subscription_name',
                 provide_dsn := 'host=nodeA dbname=postgres'
                 );

```

```

        SELECT pglogical.create_replication_set(
                 set_name := 'SmallSet');
SELECT pglogical.replication_set_add_table(
                 set_name := 'SmallSet',
                 relation := 'TableX');

```

```

        SELECT pglogical.create_subscription(
                 subscription_name := 
'SmallSet_subscription',
                 replication_sets := ARRAY['SmallSet'],
                 provide_dsn := 'host=nodeA dbname=postgres'
                 );

```

```

          SELECT pglogical.replication_set_add_table(
                 set_name := 'SmallSet',
                 relation := 'TableY',
                 row_filter := 'status = 7',
                 synchronize_data = true);    

```

```

UPDATE table
SET
 col1 = col1 + random()
,col2 = col2 + random()
WHERE key = value

```

## Bi-directional replication

```

track_commit_timestamps = on

```

```

        UPDATE foo SET col1 = col1 + 1 WHERE key = value; 

```

## Archiving transaction log data

```

       pg_receivexlog -D /pgarchive/alpha -d $MYCONNECTIONSTRING &

```
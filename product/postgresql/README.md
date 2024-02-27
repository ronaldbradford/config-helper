# PostgreSQL

|             |             |
| ----------- | ----------- |
| Product     | **PostgreSQL**   |
| Type        | Relational Database (RDBMS) |
| Website     | [https://wwww.postgresql.org](https://www.postgresql.org)|
| Created     | 2024-01-22  |


## Startup File Configuration

TBD

## Runtime Configuration

Using the `psql` client or another client you can retrieve the runtime configuration via several SQL statements:

```
psql> SHOW ALL;
```

For more information see the [PostgreSQL `SHOW` manual page](https://www.postgresql.org/docs/15/sql-show.html).

or

```
psql> SELECT name,setting FROM pg_settings;
```

For more information see the [PostgreSQL `pg_settings` manual page](https://www.postgresql.org/docs/15/view-pg-settings.html).

The `pg_settings` view contains additional attributes that are not required. These are:

```
\d pg_settings;
              View "pg_catalog.pg_settings"
    Column      |  Type   | Collation | Nullable | Default
----------------+---------+-----------+----------+---------
name            | text    |           |          |
setting         | text    |           |          |
unit            | text    |           |          |
category        | text    |           |          |
short_desc      | text    |           |          |
extra_desc      | text    |           |          |
context         | text    |           |          |
vartype         | text    |           |          |
source          | text    |           |          |
min_val         | text    |           |          |
max_val         | text    |           |          |
enumvals        | text[]  |           |          |
boot_val        | text    |           |          |
reset_val       | text    |           |          |
sourcefile      | text    |           |          |
sourceline      | integer |           |          |
pending_restart | boolean |           |          |
```

### Quick Capture

```
URL="postgres://${DBA_USER}:${DBA_PASSWD}@${HOST}:${PORT}/${DB_SCHEMA}"
psql $URL -c "SELECT name,setting FROM pg_settings" > postgres.settings.txt
```


### Example Output

Depending on the version, the output can contain 300+ different configuration settings.

```
psql> SHOW ALL;
name                                   |                     setting                     |                                                               description
---------------------------------------+-------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------
allow_in_place_tablespaces             | off                                             | Allows tablespaces directly inside pg_tblspc, for testing.
allow_system_table_mods                | off                                             | Allows modifications of the structure of system tables.
application_name                       | psql                                            | Sets the application name to be reported in statistics and logs.
archive_cleanup_command                |                                                 | Sets the shell command that will be executed at every restart point.
archive_command                        | /var/lib/postgresql/15/main/wal_backup.sh %p %f | Sets the shell command that will be called to archive a WAL file.
archive_library                        |                                                 | Sets the library that will be called to archive a WAL file.
archive_mode                           | on                                              | Allows archiving of WAL files using archive_command.
archive_timeout                        | 2min                                            | Sets the amount of time to wait before forcing a switch to the next WAL file.
array_nulls                            | on                                              | Enable input of NULL elements in arrays.
authentication_timeout                 | 1min                                            | Sets the maximum allowed time to complete client authentication.
autovacuum                             | on                                              | Starts the autovacuum subprocess.
autovacuum_analyze_scale_factor        | 0.1                                             | Number of tuple inserts, updates, or deletes prior to analyze as a fraction of reltuples.
autovacuum_analyze_threshold           | 50                                              | Minimum number of tuple inserts, updates, or deletes prior to analyze.
autovacuum_freeze_max_age              | 200000000                                       | Age at which to autovacuum a table to prevent transaction ID wraparound.
autovacuum_max_workers                 | 3                                               | Sets the maximum number of simultaneously running autovacuum worker processes.
autovacuum_multixact_freeze_max_age    | 400000000                                       | Multixact age at which to autovacuum a table to prevent multixact wraparound.
autovacuum_naptime                     | 1min                                            | Time to sleep between autovacuum runs.
autovacuum_vacuum_cost_delay           | 2ms                                             | Vacuum cost delay in milliseconds, for autovacuum.
autovacuum_vacuum_cost_limit           | -1                                              | Vacuum cost amount available before napping, for autovacuum.
autovacuum_vacuum_insert_scale_factor  | 0.2                                             | Number of tuple inserts prior to vacuum as a fraction of reltuples.
```

## Variants

### AWS RDS

### AWS RDS Aurora

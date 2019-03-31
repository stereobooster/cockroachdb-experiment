# Experiment with DBs and auto API generation

## Database

We will use Docker to run everything. Because we need to run more than one docker image and this is a toy project we will use [docker compose](https://docs.docker.com/compose/gettingstarted/).

The first service is the database. We will use [cockroach database](https://www.cockroachlabs.com/docs/stable/start-a-local-cluster-in-docker.html) (this could be PostgreSQL as well).

I created the first version of `docker-compose.yml` based on two tutorials linked above. To test run `docker-compose up` and connect http://localhost:8080/.

## External client

(optional)

[Let's connect to the database with an external client](https://www.cockroachlabs.com/docs/managed/stable/managed-connect-to-your-cluster.html).

Start compose

```
docker-compose up
```

[Use DB client of your preference](https://github.com/dhamaniasad/awesome-postgres#gui). I would use [pgcli](https://www.pgcli.com/).

```
pgcli "postgresql://root@localhost:26257/defaultdb?sslmode=disable"
Server: PostgreSQL CCL
Version: 2.0.2
Chat: https://gitter.im/dbcli/pgcli
Mail: https://groups.google.com/forum/#!forum/pgcli
Home: http://pgcli.com
root@localhost:defaultdb> Exception in thread completion_refresh:
Traceback (most recent call last):
  File "python/Frameworks/Python.framework/Versions/3.7/lib/python3.7/threading.py", line 917, in _bootstrap_inner
    self.run()
  File "python/Frameworks/Python.framework/Versions/3.7/lib/python3.7/threading.py", line 865, in run
    self._target(*self._args, **self._kwargs)
  File "pgcli/2.0.2/libexec/lib/python3.7/site-packages/pgcli/completion_refresher.py", line 68, in _bg_refresh
    refresher(completer, executor)
  File "pgcli/2.0.2/libexec/lib/python3.7/site-packages/pgcli/completion_refresher.py", line 111, in refresh_tables
    completer.extend_foreignkeys(executor.foreignkeys())
  File "pgcli/2.0.2/libexec/lib/python3.7/site-packages/pgcli/pgcompleter.py", line 259, in extend_foreignkeys
    for fk in fk_data:
  File "pgcli/2.0.2/libexec/lib/python3.7/site-packages/pgcli/pgexecute.py", line 614, in foreignkeys
    cur.execute(query)
psycopg2.NotSupportedError: unimplemented at or near ")"
DETAIL:  source SQL:

                SELECT s_p.nspname AS parentschema,
                       t_p.relname AS parenttable,
                       unnest((
                        select
                            array_agg(attname ORDER BY i)cs-mode     Refreshing completions...
                                                        ^
HINT:  See: https://github.com/cockroachdb/cockroach/issues/23620

>
```

## Sample database

Let's set up [sample database](https://stackoverflow.com/questions/5363613/sample-database-for-postgresql).

There are a lot of them (but not all work out of the box):

- https://github.com/devrimgunduz/pagila
- https://github.com/catherinedevlin/opensourceshakespeare
- http://pgfoundry.org/projects/dbsamples/
- https://musicbrainz.org/doc/MusicBrainz_Database/Download
- http://wiki.postgresql.org/wiki/Sample_Databases

I picked up [opensourceshakespeare](https://github.com/catherinedevlin/opensourceshakespeare).

```
docker-compose up -d
  Starting db-experiment_db_1 ... done
docker exec -it db-experiment_db_1 ./cockroach sql --insecure
root@:26257/defaultdb> IMPORT PGDUMP 'https://raw.githubusercontent.com/stereobooster/opensourceshakespeare/master/shakespeare.sql';
```

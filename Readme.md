# Experiment with DBs and auto API generation

We will use Docker to run everything. Because we need to run more than one docker image and this is a toy project we will use [docker compose](https://docs.docker.com/compose/gettingstarted/).

The first service is the database. We will use [cockroach database](https://www.cockroachlabs.com/docs/stable/start-a-local-cluster-in-docker.html) (this could be PostgreSQL as well).

I created the first version of `docker-compose.yml` based on two tutorials linked above. To test run `docker-compose up` and connect http://localhost:8080/.

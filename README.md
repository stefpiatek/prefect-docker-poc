# Prefect proof of concept

Trying to get the simplest version of experimenting with a dockerised run being called from prefect using local process workers. 
Covers the main areas that we'd want to be sure of before investing real effort into incorporating real workflows into prefect. 

## From `prefect-poc` sub directory

```shell
export PREFECT_SERVER_API_AUTH_STRING="admin:pass"
uv run prefect server start
```

Navigate to <http://localhost:4200/dashboard> and login using the string.

In a new screen/terminal session add a worker:

```shell
export PREFECT_API_URL=http://localhost:4200/api
export PREFECT_API_AUTH_STRING="admin:pass"
uv run prefect worker start --pool 'poc-worker' --type process --limit 2
```

In another screen/terminal, load config for three versions of the `run_docker` flow:

```shell
export PREFECT_API_AUTH_STRING="admin:pass"
export PREFECT_API_URL=http://localhost:4200/api
uv run python src/prefect_poc/run_docker.py
```

Now that we have the flow configuration, deploy them. You should see some concurrency limit hit in the logs 

```shell
export PREFECT_API_AUTH_STRING="admin:pass"
export PREFECT_API_URL=http://localhost:4200/api
uv run prefect deployment run 'run-docker/first'
sleep 5 
uv run prefect deployment run 'run-docker/second'
sleep 5 
uv run prefect deployment run 'run-docker/third'
```

Add another worker to the worker pool

```shell
export PREFECT_API_URL=http://localhost:4200/api
export PREFECT_API_AUTH_STRING="admin:pass"
uv run prefect worker start --pool 'poc-worker' --type process --limit 2
```
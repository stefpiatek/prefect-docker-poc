

# From `prefect-poc`

```shell
uv run prefect server start
```

Navigate to <http://localhost:4200/dashboard>


```shell
export PREFECT_API_URL=http://localhost:4200/api
uv run prefect worker start --pool 'poc-worker' --type process --limit 2
```

```shell
uv run python src/prefect_poc/run_docker.py
```

Now the config has been loaded in, you can set off runs

```shell
uv run prefect deployment run 'run-docker/first'
sleep 5 
uv run prefect deployment run 'run-docker/second'
sleep 5 
uv run prefect deployment run 'run-docker/third'
```

Add another worker to the worker pool

```shell
export PREFECT_API_URL=http://localhost:4200/api
uv run prefect worker start --pool 'poc-worker' --type process --limit 2
```
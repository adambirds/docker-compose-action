# Docker Compose Action

This action runs your docker-compose file, allows you to run tests and cleans up before action finished. This is a copy of the repository from adambirds, but with dependencies updated.

## Inputs

### `compose-file`

**Optional** The name of the compose file. Default `"./docker-compose.yml"`.

It can be a list of files:

```yml
compose-file: |
  docker-compose.yml
  docker-compose.ci.yml
```

### `services`

**Optional** Just perform `docker-compose up` to one service instead of all of them

### `up-flags`

**Optional** Used to specify flags to pass to the `docker-compose up`. Default is none. Can be used to pass the `--build` flag, for example, if you want persistent volumes to be deleted as well during cleanup. A full list of flags can be found in the [docker-compose up documentation](https://docs.docker.com/compose/reference/up/).

### `down-flags`

**Optional** Used to specify flags to pass to the `docker-compose down` command during cleanup. Default is none. Can be used to pass the `--volumes` flag, for example, if you want persistent volumes to be deleted as well during cleanup. A full list of flags can be found in the [docker-compose down documentation](https://docs.docker.com/compose/reference/down/).

### `compose-flags`

**Optional** Used to specify flags to pass to the `docker-compose` command. Default is none. A full list of flags can be found in the [docker-compose documentation](https://docs.docker.com/compose/reference/#command-options-overview-and-help).

### `test-container`

**Optional** Used to specify the container to run the tests in. Default is none. If not specified, no tests will be run.

### `test-command`

**Optional** Used to specify the command to run the tests with. Default is none. If not specified, no tests will be run.

## Example usage

```yaml
steps:
  # need checkout before using docker-compose-action
  - uses: actions/checkout@v3
  - uses: jmalvarezf-lmes/docker-compose-action@v1.4.0
    with:
      compose-file: "./docker/docker-compose.yml"
      down-flags: "--volumes"
      services: |
        helloworld2
        helloworld3
      test-container: helloworld
      test-command: "npm test"
```

### Using environment variables

```yaml
steps:
  - uses: actions/checkout@v3
  - uses: jmalvarezf-lmes/docker-compose-action@v1.4.0
    with:
      compose-file: "./docker/docker-compose.yml"
    env:
      CUSTOM_VARIABLE: "test"
```

### Run tests on multiple containers

```yaml
steps:
  - uses: actions/checkout@v3
  - uses: jmalvarezf-lmes/docker-compose-action@v1.4.0
    with:
      compose-file: "./docker/docker-compose.yml"
      test-container: "container1"
      test-command: "npm test"

  - uses: jmalvarezf-lmes/docker-compose-action@v1.4.0
    with:
      compose-file: "./docker/docker-compose.yml"
      test-container: "container2"
      test-command: "npm test"

  - uses: jmalvarezf-lmes/docker-compose-action@v1.4.0
    with:
      compose-file: "./docker/docker-compose.yml"
      test-container: "container3"
      test-command: "npm test"
```

# Docker Compose Action

This GitHub Action allows you to run your `docker-compose` files, specify services to bring up, execute tests in containers, and clean up afterward. It simplifies running tests within a containerized environment in your CI/CD pipelines.

## Inputs

### `compose-file`

**Optional** - The path to the Docker Compose file(s). Default: `"./docker-compose.yml"`.

You can provide a list of files if needed:

```yaml
compose-file: |
  docker-compose.yml
  docker-compose.ci.yml
```

### `services`

**Optional** - The specific services to bring up with `docker-compose up`. If not specified, all services in the compose file(s) will be started.

### `up-flags`

**Optional** - Additional flags to pass to the `docker-compose up` command. Default is none. Useful for passing options like `--build` or `--force-recreate`.

### `down-flags`

**Optional** - Flags to pass to the `docker-compose down` command during cleanup. Default is none. For example, use `--volumes` if you want to remove volumes.

### `compose-flags`

**Optional** - General flags to pass to the `docker-compose` command. Default is none. These can be used to add global options like `--compatibility` or other Docker Compose features.

### `test-container`

**Optional** - The name of the container in which to run the tests. If not specified, no tests will be executed.

### `test-command`

**Optional** - The command used to run the tests within the container. For example, `npm test` for Node.js projects. If not specified, no tests will be run.

### `annotate`

**Optional** - Whether to show the console output of the test run as GitHub Action annotations. Default: `"true"`.

## Example usage

```yaml
steps:
  # Check out the repository
  - uses: actions/checkout@v4

  # Run Docker Compose Action
  - uses: adambirds/docker-compose-action@v1.5.0
    with:
      compose-file: "./docker/docker-compose.yml"
      up-flags: "--build"
      down-flags: "--volumes"
      test-container: "test-container"
      test-command: "npm test"
```

### Using environment variables

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: adambirds/docker-compose-action@v1.5.0
    with:
      compose-file: "./docker/docker-compose.yml"
    env:
      CUSTOM_VARIABLE: "test"
```

### Running tests on multiple containers

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: adambirds/docker-compose-action@v1.5.0
    with:
      compose-file: "./docker/docker-compose.yml"
      test-container: "container1"
      test-command: "npm test"

  - uses: adambirds/docker-compose-action@v1.5.0
    with:
      compose-file: "./docker/docker-compose.yml"
      test-container: "container2"
      test-command: "npm test"

  - uses: adambirds/docker-compose-action@v1.5.0
    with:
      compose-file: "./docker/docker-compose.yml"
      test-container: "container3"
      test-command: "npm test"
```

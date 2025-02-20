<!-- markdownlint-disable-next-line first-line-heading -->
<div align="center" width="100%">
<!-- start branding -->

<img src=".github/ghadocs/branding.svg" width="15%" align="center" alt="branding<icon:anchor color:gray-dark>" />

<!-- end branding -->
<!-- start title -->

# <img src=".github/ghadocs/branding.svg" width="60px" align="center" alt="branding<icon:anchor color:gray-dark>" /> GitHub Action: Docker Compose Action

<!-- end title -->
<!-- start badges -->

<a href="https%3A%2F%2Fgithub.com%2Fhoverkraft-tech%2Fcompose-action%2Freleases%2Flatest"><img src="https://img.shields.io/github/v/release/shining-so/compose-action?display_name=tag&sort=semver&logo=github&style=flat-square" alt="Release%20by%20tag" /></a><a href="https%3A%2F%2Fgithub.com%2Fhoverkraft-tech%2Fcompose-action%2Freleases%2Flatest"><img src="https://img.shields.io/github/release-date/shining-so/compose-action?display_name=tag&sort=semver&logo=github&style=flat-square" alt="Release%20by%20date" /></a><img src="https://img.shields.io/github/last-commit/shining-so/compose-action?logo=github&style=flat-square" alt="Commit" /><a href="https%3A%2F%2Fgithub.com%2Fhoverkraft-tech%2Fcompose-action%2Fissues"><img src="https://img.shields.io/github/issues/shining-so/compose-action?logo=github&style=flat-square" alt="Open%20Issues" /></a><img src="https://img.shields.io/github/downloads/shining-so/compose-action/total?logo=github&style=flat-square" alt="Downloads" />

<!-- end badges -->

</div>
<!-- start description -->

This action runs your compose file(s) and clean up before action finished

<!-- end description -->

<!-- start contents -->
<!-- end contents -->

## Usage

### Action

The action will run `docker compose up` to start the services defined in the given compose file(s).
The compose file(s) can be specified using the `compose-file` input.
Some extra options can be passed to the `docker compose up` command using the `up-flags` input.

### Post hook

On post hook, the action will run `docker compose down` to clean up the services.
In [debug mode](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/troubleshooting-workflows/enabling-debug-logging), the logs of the running services are printed before the cleanup.

Some extra options can be passed to the `docker compose down` command using the `down-flags` input.

<!-- start usage -->

```yaml
- uses: shining-so/compose-action@v2.3.1
  with:
    # Description: Additional options to pass to `docker` command.
    #
    docker-flags: ""

    # Description: Path to compose file(s). It can be a list of files. It can be
    # absolute or relative to the current working directory (cwd).
    #
    # Default: ./docker-compose.yml
    compose-file: ""

    # Description: Services to perform docker compose up.
    #
    services: ""

    # Description: Additional options to pass to `docker compose up` command.
    #
    # Default:
    up-flags: ""

    # Description: Additional options to pass to `docker compose down` command.
    #
    # Default:
    down-flags: ""

    # Description: Additional options to pass to `docker compose push` command.
    #
    # Default:
    push-flags: ""

    # Description: Additional options to pass to `docker compose` command.
    #
    # Default:
    compose-flags: ""

    # Description: Current working directory
    #
    # Default: ${{ github.workspace }}
    cwd: ""

    # Description: Compose version to use. If null (default), it will use the current
    # installed version. If "latest", it will install the latest version.
    #
    compose-version: ""

    # Description: The GitHub token used to create an authenticated client (to fetch
    # the latest version of docker compose).
    #
    # Default: ${{ github.token }}
    github-token: ""
```

<!-- end usage -->

## Inputs

<!-- start inputs -->

| **Input**                    | **Description**                                                                                                                                  | **Default**                          | **Required** |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------ | ------------ |
| <code>docker-flags</code>    | Additional options to pass to <code>docker</code> command.                                                                                       |                                      | **false**    |
| <code>compose-file</code>    | Path to compose file(s). It can be a list of files. It can be absolute or relative to the current working directory (cwd).                       | <code>./docker-compose.yml</code>    | **false**    |
| <code>services</code>        | Services to perform docker compose up.                                                                                                           |                                      | **false**    |
| <code>up-flags</code>        | Additional options to pass to <code>docker compose up</code> command.                                                                            |                                      | **false**    |
| <code>down-flags</code>      | Additional options to pass to <code>docker compose down</code> command.                                                                          |                                      | **false**    |
| <code>push-flags</code>      | Additional options to pass to <code>docker compose push</code> command.                                                                          |                                      | **false**    |
| <code>compose-flags</code>   | Additional options to pass to <code>docker compose</code> command.                                                                               |                                      | **false**    |
| <code>cwd</code>             | Current working directory                                                                                                                        | <code>${{ github.workspace }}</code> | **false**    |
| <code>compose-version</code> | Compose version to use.<br />If null (default), it will use the current installed version.<br />If "latest", it will install the latest version. |                                      | **false**    |
| <code>github-token</code>    | The GitHub token used to create an authenticated client (to fetch the latest version of docker compose).                                         | <code>${{ github.token }}</code>     | **false**    |

<!-- end inputs -->
<!-- start outputs -->
<!-- end outputs -->

## Examples

### Example using in a full workflow

```yaml
name: Docker Compose Action

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Run docker compose
        uses: shining-so/compose-action@v2.3.1
        with:
          compose-file: "./docker/docker-compose.yml"

      - name: Execute tests in the running services
        run: |
          docker compose exec test-app pytest
```

<!-- start [.github/ghadocs/examples/] -->
<!-- end [.github/ghadocs/examples/] -->

### Example Using environment variables

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: shining-so/compose-action@v2.3.1
    with:
      compose-file: "./docker/docker-compose.yml"
    env:
      CUSTOM_VARIABLE: "test"
```

### Example using `services`

Perform `docker compose up` to some given service instead of all of them

```yaml
steps:
  # need checkout before using compose-action
  - uses: actions/checkout@v3
  - uses: shining-so/compose-action@v2.3.1
    with:
      compose-file: "./docker/docker-compose.yml"
      services: |
        helloworld2
        helloworld3
```

### Example using `up-flags`

Specify flags to pass to the `docker compose up`. Default is none. Can be used
to pass the `--build` flag, for example, if you want persistent volumes to be
deleted as well during cleanup. A full list of flags can be found in the
[docker compose up documentation](https://docs.docker.com/compose/reference/up/).

### Example using `down-flags`

Specify flags to pass to the `docker compose down` command during cleanup.
Default is none. Can be used to pass the `--volumes` flag, for example, if you
want persistent volumes to be deleted as well during cleanup. A full list of
flags can be found in the
[docker compose down documentation](https://docs.docker.com/compose/reference/down/).

### Example using `compose-flags`

Specify flags to pass to the `docker compose` command. Default is none. A full
list of flags can be found in the
[docker compose documentation](https://docs.docker.com/compose/reference/#command-options-overview-and-help).

```yaml
steps:
  # need checkout before using compose-action
  - uses: actions/checkout@v3
  - uses: shining-so/compose-action@v2.3.1
    with:
      compose-file: "./docker/docker-compose.yml"
      services: |
        helloworld2
        helloworld3
```

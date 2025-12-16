# macOS (Apple Silicon) Setup for Local Development

This document describes the necessary modifications to run Swarmpit on a macOS environment with Apple Silicon (e.g., M1, M2).

The main issue is that the default Docker API version used by Swarmpit is outdated for recent Docker Desktop versions on macOS.

## Modifications

To resolve the "client version is too old" error, you need to update the Docker API version in the following files:

1.  **`docker-compose.yml` and `docker-compose.arm.yml`**:
    Set the `SWARMPIT_DOCKER_API` environment variable for the `app` service to `1.44`.

    ```yaml
    services:
      app:
        environment:
          - SWARMPIT_DOCKER_API=1.44
    ```

2.  **`dev/script/init-agent.sh`**:
    Update the `DOCKER_API_VERSION` environment variable to `1.44`.

    ```bash
    docker run -d \
      # ...
      --env DOCKER_API_VERSION=1.44 \
      # ...
    ```

3.  **`dev/script/start-dind.sh`**:
    Export `DOCKER_API_VERSION` as `1.44`.

    ```bash
    export DOCKER_API_VERSION=1.44
    ```

By applying these changes, you should be able to build and deploy Swarmpit on your macOS machine.

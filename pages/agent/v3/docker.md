# Running Buildkite Agent with Docker

You can run the Buildkite Agent inside a Docker container using the [official image on Docker Hub](https://hub.docker.com/r/buildkite/agent).

>📘 Running each build in its own container
> These instructions cover how to run the agent using Docker. If you want to learn how to isolate each build using Docker and any of our standard Linux-based installers read the <a href="/docs/pipelines/tutorials/docker-containerized-builds">Containerized builds with Docker</a> guide.

## Running using Docker

Start an agent with the [official image](https://hub.docker.com/r/buildkite/agent/) based on Alpine Linux:

```shell
docker run -d -t --name buildkite-agent buildkite/agent:3 start --token "<your-agent-token>"
```

A much larger Ubuntu-based image is also available:

```shell
docker run -d -t --name buildkite-agent buildkite/agent:3-ubuntu start --token "<your-agent-token>"
```

>🚧 Caveats for builds that need Docker access.
> If your build jobs require Docker access, and you're passing through the Docker socket, you must ensure the build path is consistent between the Docker host and the agent container. See <a href="#allowing-builds-to-use-docker">Allowing builds to use Docker</a> for more details.

## Version tagging

The default tag (`buildkite/agent:latest`) will always point to the latest stable release, but we recommend you use `buildkite/agent:3` to prevent breaking changes. If you want to use an exact version, you can use the corresponding tag, such as `buildkite/agent:3.0.1`. See [Docker Hub](https://hub.docker.com/r/buildkite/agent/tags/) for a list of all the available versions.

## Default file locations

* Configuration: `/buildkite/buildkite-agent.cfg`
* Agent Hooks: `/buildkite/hooks`
* Builds: `/buildkite/builds`
* Agent user home: `/root`

## Configuration

Most [agent configuration settings](/docs/agent/configuration) can be set with environment variables. You can also mount a configuration file in, for example:

```bash
docker run \
  -v "/path/to/buildkite-agent.cfg:/buildkite/buildkite-agent.cfg:ro" \
  -d \
  -t \
  --name buildkite-agent \
  buildkite/agent:3 start --token "<your-agent-token>"
```

## Which user the agent runs as

On Docker, the default user is `root`, unless you use `docker run --user`, or use a Kubernetes Pod security context to override the user

## Adding hooks

You can add [custom agent hooks](/docs/agent/hooks) by mounting or copying them into the `/buildkite/hooks` directory, and ensuring they are executable.

For example, this is how you'd mount the hooks directory using a read-only host volume:

```bash
docker run \
  -v "/path/to/buildkite-hooks:/buildkite/hooks:ro" \
  -d \
  -t \
  --name buildkite-agent \
  buildkite/agent:3 start --token "<your-agent-token>"
```

Alternatively, if you create your own image based off `buildkite/agent`, you can copy your hooks into the correct location:

```dockerfile
FROM buildkite/agent:3

COPY hooks /buildkite/hooks/
```

## Permissions errors when using Docker

A problem you may encounter when using Docker volume mounts (-v) in Linux or Windows is that the container may create files on the host system with root user permissions. This can result in errors like the following:

```
$ git clean -fxdq
warning: failed to remove dist/
```

Permissions on the host are set based on the user running the Docker daemon, which under Linux is generally `root`. When the Agent (running as `buildkite-agent`) tries to subsequently remove or modify those files, permissions errors occur.

To ensure correct file permissions, you can:

* Change the way permissions are set on the files created by your Docker container: modify your container's `USER` or modify your build commands.

* Configure [user namespace remapping](https://docs.docker.com/engine/security/userns-remap/) on your Docker host to ensure that container users are remapped to the same user running your `buildkite-agent`.

* Run a script before or after builds that resets permissions. You can do this either using Docker (because it runs as root) or using `sudo`. See the Buildkite Elastic CI Stack for AWS's [fix-buildkite-agent-builds-permissions](https://github.com/buildkite/elastic-ci-stack-for-aws/blob/v2.3.4/packer/conf/buildkite-agent/scripts/fix-buildkite-agent-builds-permissions) script or the [sudoers.conf](https://github.com/buildkite/elastic-ci-stack-for-aws/blob/v2.3.4/packer/conf/buildkite-agent/sudoers.conf) script for examples of using an agent hook and sudo command to reset permissions.

## Allowing builds to use Docker

To use Docker and volume mounting from build scripts, you need to ensure that the builds directory, and the `buildkite-agent` binary path, are mounted in from the host machine, and that their paths on the host and in the agent container are the same.

For example, when a script runs the command `docker run --volume "$PWD:/code" ...` the `$PWD` environment variable will resolve to the path in your agent's container (for example, `/var/lib/buildkite/builds/my-org/my-pipeline`). The Docker daemon, which exists on the host machine, will attempt to mount `/var/lib/buildkite/builds/my-org/my-pipeline` from the host file system, not the agent container file system. If that directory does not exist on the host, Docker will mount an empty directory to `/code` without showing any error.

The following example shows how to configure the agent container with the correct host volumes mounts, and `BUILDKITE_BUILD_PATH` configuration:

```bash
docker run \
  -v "/var/lib/buildkite/builds:/var/lib/buildkite/builds" \
  -v "/usr/local/bin/buildkite-agent:/usr/local/bin/buildkite-agent" \
  -v "/var/run/docker.sock:/var/run/docker.sock" \
  -e "BUILDKITE_BUILD_PATH=/var/lib/buildkite/builds" \
  -d \
  -t \
  --name buildkite-agent \
  buildkite/agent:3 start --token "<your-agent-token>"
```

>🚧 Security considerations
> Providing builds with a Docker socket gives them access to whatever the docker daemon has access to on the host system. Typically this is <code>root</code>, which means builds have full root system access. This can be mitigated somewhat with <a href="https://docs.docker.com/engine/security/userns-remap/">user namespace remapping</a>, but caution should still be exercised.

## Exposing build secrets into the container

There are many approaches to exposing secrets to Docker containers. In addition, many Docker platforms have their own methods for exposing secrets. If you're running your own Docker containers, we recommend using a read-only [host volume](https://docs.docker.com/engine/tutorials/dockervolumes/#mount-a-host-directory-as-a-data-volume).

The following example mounts a directory containing secrets on the host machine (`$HOME/buildkite-secrets`) into the container as a read-only data volume at `/buildkite-secrets`:

```bash
docker run \
  -v "/path/to/buildkite-secrets:/buildkite-secrets:ro" \
  -d \
  -t \
  --name buildkite-agent \
  buildkite/agent:3 start --token "<your-agent-token>"
```

If you've exposed pipeline secrets as environment variables, you can pass them through to the container using the -e option:

``` bash
docker run \
  -e MY_SECRET_ENV \
  -d \
  -t \
  --name buildkite-agent \
  buildkite/agent:3 start --token "<your-agent-token>"
```

## Docker Hub rate limits

If you're using Docker with Docker images hosted on Docker Hub, note that as of 2nd November 2020 there are [strict rate limits](/docs/pipelines/integrations/other/docker-hub) for image downloads.

## Authenticating private git repositories

To configure a [git-credentials file](https://git-scm.com/docs/git-credential-store#_storage_format) located at `/buildkite-secrets/git-credentials`, you could use the following
[`environment` hook](/docs/agent/hooks#job-lifecycle-hooks) mounted to `/buildkite/hooks/environment`:

```bash
#!/bin/bash

set -euo pipefail

git config --global credential.helper "store --file=/buildkite-secrets/git-credentials"

# You can export other secrets here too
# export FOO=bar
```

To configure a private SSH key located at `/buildkite-secrets/id_rsa_buildkite_git`
you could use the following [`environment` hook](/docs/agent/hooks#job-lifecycle-hooks)
mounted to `/buildkite/hooks/environment`:

```bash
#!/bin/bash

set -euo pipefail

eval "$(ssh-agent -s)"
ssh-add -k /buildkite-secrets/id_rsa_buildkite_git

# You can export other secrets here too
# export FOO=bar
```

Other options for configuring Git and SSH include:

* Running `ssh-agent` on the host machine and mounting the ssh-agent socket into the containers. See the [Buildkite Agent SSH keys documentation](/docs/agent/ssh-keys) for examples on using ssh-agent.
* The least-secure approach: the built-in [docker-ssh-env-config](https://github.com/buildkite/docker-ssh-env-config) support allows you to pass in keys using environment variables.

## Entrypoint customizations

The entrypoint uses `tini` to correctly pass signals to, and kill, sub-processes. Instead of redefining `ENTRYPOINT` we recommend you copy executable scripts into `/docker-entrypoint.d/`. All executable scripts should not contain any file extension, and will be executed in alphanumeric order.

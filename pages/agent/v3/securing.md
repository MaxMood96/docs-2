# Securing your Buildkite Agent

In cases where a Buildkite Agent is being deployed into a sensitive environment, there are a few default settings which may be adjusted and techniques that may be used.

## Securely storing secrets

For best practices and recommendations about secret storage in the Agent, see [Managing pipeline secrets](/docs/pipelines/security/secrets/managing).

Learn more about other secrets management approaches in Buildkite on the [Secrets overview](/docs/pipelines/security/secrets) page.

## Set the agent token expiration date

For secure and automated agent token lifecycle management, you can use Buildkite's APIs to set the expiration date for agent tokens. Learn more about this feature in [Agent token lifetime](/docs/agent/v3/tokens#agent-token-lifetime). This feature allows for automated token rotation for long-lived tokens. Once set, an agent token's expiration date cannot be changed.

## Disable automatic ssh-keyscan

By default the agent will automatically accept the Git SSH host using the `ssh-keyscan` command when doing the first checkout on a new agent host. The agent runs a similar command to this:

```bash
ssh-keyscan "<host>" >> "~/.ssh/known_hosts"
```

If you choose to disable this functionality, you'll need to manually perform your first checkout, or ensure the SSH fingerprint of your source code host is already present on your build machine.

Automatic ssh-keyscan can be disabled by setting [`no-ssh-keyscan`](/docs/agent/v3/configuration#no-ssh-keyscan):

- Environment variable: `BUILDKITE_NO_SSH_KEYSCAN=true`
- Command line flag: `--no-ssh-keyscan`
- Configuration setting: `no-ssh-keyscan=true`

## Restrict access by the Buildkite agent controller

To safeguard your organization's infrastructure in case of Buildkite infrastructure being compromised, you want to restrict what the agent can and cannot do. All of the information above about securing the Buildkite agent applies, specifically:

- Disallow execution of arbitrary commands by [customizing the list of allowed plugins](#restrict-access-by-the-buildkite-agent-controller-allow-a-list-of-plugins) or [disabling plugins](#restrict-access-by-the-buildkite-agent-controller-disable-plugins) entirely
- [Disable command evaluation](#restrict-access-by-the-buildkite-agent-controller-disable-command-evaluation)
- [Disable local hooks](#restrict-access-by-the-buildkite-agent-controller-disable-local-hooks)
- [Enable strict validation](#restrict-access-by-the-buildkite-agent-controller-strict-checks-using-a-pre-bootstrap-hook)

As a result, your Buildkite agent will refuse to run anything that's not a single argumentless invocation of a script that exists locally (after the `git clone` step of the setup) unless it's explicitly allowed by you.
Since the [agent](https://github.com/buildkite/agent) is open-source, if necessary you can verify that assertion to whatever degree of certainty is required.

### Allow a list of plugins

Defining an [environment hook](hooks#job-lifecycle-hooks) in the
[agent `hooks-path`](hooks#hook-locations-agent-hooks), you can create a
list of plugins that an agent is allowed to run by inspecting the
`BUILDKITE_PLUGINS` [environment variable](/docs/pipelines/configure/environment-variables).
For an example of this, see the [buildkite/buildkite-allowed-plugins-hook-example](https://github.com/buildkite/buildkite-allowed-plugins-hook-example)
repository on GitHub.

### Disable plugins

As plugins execute in the same way as local hooks, they can pose a potential security risk. If you're using third party plugins, you'll be executing the third party's code on your agent.

You can disable plugins with the command line flag: `--no-plugins` or the [`no-plugins`](/docs/agent/v3/configuration#no-plugins) setting.

If you still want to use plugins, you can check out a tool for [signing pipelines](/docs/agent/v3/securing#sign-pipelines).

### Disable command evaluation

By default the agent allows you to run any command on the build server (for example, `make test`). You can disable command evaluation and allow only the execution of scripts (with no ability to pass command line flags). Disabling command evaluation will also disable plugin execution.

Once disabled your build steps will need to be checked into your repository as scripts, and the only way to pass arguments is using environment variables.


This option is intended to protect your infrastructure from a scenario where Buildkite itself gets compromised, and subsequently sends malicious commands to your agents. It is not designed, nor effective at protecting against malicious actors with commit access to your repositories.

Command line evaluation can be disabled by setting [`no-command-eval`](/docs/agent/v3/configuration#no-command-eval):

- Environment variable: `BUILDKITE_NO_COMMAND_EVAL=1`
- Command line flag: `--no-command-eval`
- Configuration setting: `no-command-eval=true`

> 🚧 Custom hooks and environment variables
> If you have a custom `command` hook, using `no-command-eval` will have no effect on your command execution. See [Allowing a list of plugins](#restrict-access-by-the-buildkite-agent-controller-allow-a-list-of-plugins) and [Custom bootstrap scripts](#customize-the-bootstrap) for examples of how to completely lock down your agent from arbitrary code execution.
> <p>
> Using `no-command-eval` only prevents command evaluation by the agent itself. Other programs such as build or test tools that run during the job could be influenced into executing arbitrary commands via environment variables (for example, `BASH_ENV` or `GIT_SSH_COMMAND`). See [Strict checks using a pre-bootstrap hook](#restrict-access-by-the-buildkite-agent-controller-strict-checks-using-a-pre-bootstrap-hook) and [`enable-environment-variable-allowlist`](/docs/agent/v3/cli-start#enable-environment-variable-allowlist) for possible approaches to filtering environment variables.

### Disable local hooks

Local hooks are hooks defined in pipeline's repository.

If you have enforced a security policy using agent hooks (for example, you force commands to
run within a Docker container or a chroot environment), you should also disable
local hooks so that your security measures cannot be evaded with local hooks.
Disabling local hooks also disables plugins from all sources.

You can disable local hooks with the [`no-local-hooks`](/docs/agent/v3/configuration#no-local-hooks)
setting.

If local hooks are disabled and one is in the checkout, the job will fail.

>🚧 Building untrusted commits
>If you build untrusted commits, be careful to contain the build scripts and anything else that may be influenced by the repository contents within chroots, containers, VMs, etc as is appropriate for your needs.

### Strict checks using a pre-bootstrap hook

You can use a [`pre-bootstrap` hook](hooks#agent-lifecycle-hooks) to add strict
checks for which repositories, commands, and plugins are allowed to run on your
agent. The `pre-bootstrap` hook is executed before any source code is checked
out, and before any commands are executed.

For example, the following `pre-bootstrap` hook allows only a single file from a
single repository to be executed by the agent:

```bash
#!/bin/bash
set -euo pipefail

for line in "$(grep "^BUILDKITE_REPO=" "${BUILDKITE_ENV_FILE}")"
do
  repo="$(echo "${line}" | cut -d= -f2 | sed -e 's/^"//' -e 's/"$//')"
  if [ "${repo}" != "git@server:repo.git" ]
  then
    echo "Repository not allowed: ${repo}"
    exit 1
  fi
done

for line in "$(grep "^BUILDKITE_COMMAND=" "${BUILDKITE_ENV_FILE}")"
do
  command="$(echo "${line}" | cut -d= -f2 | sed -e 's/^"//' -e 's/"$//')"
  if [ "${command}" != "some-script.sh" ]
  then
    echo "Command not allowed: ${command}"
    exit 1
  fi
done
```

You can see from the previous example that `$BUILDKITE_ENV_FILE` is the location of file that contains the environment variables that the control plane passes to a job. You may use this to block jobs from executing if certain environment variables are set. For example, the following `pre-bootstrap` hook blocks a job from executing if the `ENVIRONMENT_VARIABLE_TO_DENY` environment variable is set.

```bash
#!/bin/bash

set -euo pipefail

if grep '^ENVIRONMENT_VARIABLE_TO_DENY=' "$BUILDKITE_ENV_FILE" > /dev/null
then
  echo "Rejecting job because the environment variable ENVIRONMENT_VARIABLE_TO_DENY has been set"
  exit 1
fi
```

But also remember that some [environment variables may be essential](/docs/pipelines/configure/environment-variables) to the execution of jobs, so adding them to a blocklist in this manner is not advisable.

## Sign pipelines

You can sign the steps your pipeline runs for extra security. This allows the agent to verify that the steps it runs haven't been tampered with or smuggled from one pipeline to another. For more information, see [Signed pipelines](/docs/agent/v3/signed-pipelines).

## Customize the bootstrap

The Buildkite Agent comes with a default bootstrap handler, but can be
[configured](/docs/agent/v3/configuration#bootstrap-script) to run your own
instead. Providing your own bootstrap provides the highest level of security and
control of your agent. You can use it to customize your agent, sanitize command
output, and implement your own security logic.

The Buildkite Agent is separated into a daemon executable and a bootstrap
executable. The daemon is responsible for communicating with the Buildkite API
and executing the bootstrap for each assigned job. The bootstrap is responsible
for checking out source code, calling hooks, running commands, and uploading
build artifacts. The bootstrap is passed environment variables by the daemon
process, and has its output streams and exit status captured.

For example, the following custom bootstrap will print out the environment
variables passed by the main agent process, print a hello world, and exit with a
failure status:

```bash
#!/bin/bash
set -euo pipefail

env

echo "Hello world"

exit 1
```

## Force clean checkouts

By default, Buildkite will reuse (after cleaning) a previous checkout. This may be unsafe if building commits from untrusted sources (for example, third-party pull requests). To force a clean checkout every time, set `BUILDKITE_CLEAN_CHECKOUT=true` in the environment. The following example shows how to enforce a clean checkout at the step level:

```yaml
steps:
- label: "Clean Checkout"
  command: echo "clean checkout"
  env:
    BUILDKITE_CLEAN_CHECKOUT: true
```

In the logs for this step, you will find a log group called "Cleaning pipeline checkout."

## Run the agent behind a proxy

To run the agent behind a proxy, you'll need to export the following proxy environment variables for your process manager:

- `http_proxy`
- `https_proxy`

Both of these variables should be set to the URL for your proxy server.

For example, if using systemd, create a directory named `/etc/systemd/system/buildkite-agent.service.d` that contains a `proxy.conf` file.

An example systemd `proxy.conf` file:

```
[Service]
# Proxy Env Vars
Environment=http_proxy=http://username:password@proxyserver:8080/
Environment=https_proxy=http://username:password@proxyserver:8080/
```
{: codeblock-file="proxy.conf"}

After creating this file, systemd will require a reload and the `buildkite-agent` service will require a restart.

## Restrict agent connection by IP address

[Clusters](/docs/pipelines/clusters) provide a mechanism to restrict which IP addresses can connect using a given agent token. This protects against the misuse of agent tokens and the hijacking of agent sessions.

To restrict agent connection by IP address, set the [**Allowed IP Addresses** attribute](/docs/pipelines/clusters/manage-clusters#restrict-an-agent-tokens-access-by-ip-address). This restricts agent registration to those IPs, and any existing agents outside the allowed IP ranges will be forcefully disconnected.

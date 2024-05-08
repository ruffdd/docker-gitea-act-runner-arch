# ruffdd/docker-gitea-act-runner-arch 
[![License](https://img.shields.io/github/license/vegardit/docker-gitea-act-runner.svg?label=license)](#license)

1. [What is it?](#what-is-it)
1. [Usage](#usage)
1. [License](#license)


## <a name="what-is-it"></a>What is it?

`archlinux` based Docker image containing [Gitea](https://gitea.com)'s [act_runner](https://gitea.com/gitea/act_runner/). Forked of [vegardit/gitea-act-runner](https://github.com/vegardit/docker-gitea-act-runner)

## <a name="usage"></a>Usage

### Additional environment variables

The following environment variables can be specified to further configure the service.

#### Runner registration:
Name|Default Value|Description
----|-------------|-----------
GITEA_INSTANCE_INSECURE|`false`|It `true` don't verify the TLS certificate of the Gitea instance
GITEA_RUNNER_NAME|`<empty>`|If not specified the container's hostname is used
GITEA_RUNNER_LABELS|`<empty>`|Comma-separated list of labels in the format of `label[:schema[:args]]`. If not specified the following labels are used `ubuntu-latest:docker://catthehacker/ubuntu:act-22.04,ubuntu-22.04:docker://catthehacker/ubuntu:act-22.04,ubuntu-20.04:docker://catthehacker/ubuntu:act-20.04`
GITEA_RUNNER_REGISTRATION_FILE|`/data/.runner`|The JSON file that holds the result from the runner registration with the Gitea instance
GITEA_RUNNER_REGISTRATION_TIMEOUT|`30`|In case of failure, registration is retried until this timeout in seconds is reached
GITEA_RUNNER_REGISTRATION_RETRY_INTERVAL|`5`|Wait period in seconds between registration retries

#### Runner runtime config:

Name|Default Value|Description
----|-------------|-----------
GITEA_RUNNER_CONFIG_TEMPLATE_FILE|`/opt/config.template.yaml`|Template to derive the effective config file from, see [image/config.template.yaml](image/config.template.yaml)
GITEA_RUNNER_UID|`1000`|The UID of the Gitea runner process
GITEA_RUNNER_GID|`1000`|The GID of the Gitea runner process
GITEA_RUNNER_LOG_EFFECTIVE_CONFIG|`false`|If set to true logs the effective YAML configuration to stdout during startup.

#### Runner config template variables

The following environment variables are referenced in the `/opt/config.template.yaml` file.

Name|Default Value|Description
----|-------------|-----------
GITEA_RUNNER_LOG_LEVEL|`info`|The level of logging, can be trace, debug, info, warn, error, fatal
GITEA_RUNNER_ENV_FILE|`/data/.env`|Extra environment variables to run jobs from a file
GITEA_RUNNER_FETCH_TIMEOUT|`5s`|The timeout for fetching the job from the Gitea instance
GITEA_RUNNER_FETCH_INTERVAL|`2s`|The interval for fetching the job from the Gitea instance
GITEA_RUNNER_MAX_PARALLEL_JOBS|`1`|Maximum number of concurrently executed jobs
GITEA_RUNNER_JOB_CONTAINER_DOCKER_HOST|`<empty>`|If empty, the available docker host is located automatically. If set to `-`, the available docker host is located automatically, but the docker host won't be mounted to the job containers. If it's any other value, the specified docker host will be used.
GITEA_RUNNER_JOB_CONTAINER_NETWORK|`bridge`|Docker network to use with job containers. Can be `bridge`, `host`, `none`, or the name of a custom network
GITEA_RUNNER_JOB_CONTAINER_PRIVILEGED|`false`|Whether to run jobs in containers with privileged mode which is required for **Docker-in-Docker** aka **dind**
GITEA_RUNNER_JOB_CONTAINER_OPTIONS|`<empty>`|Additional container launch options (eg, --add-host=my.gitea.url:host-gateway)
GITEA_RUNNER_JOB_CONTAINER_WORKDIR_PARENT|`/workspace`|The parent directory of a job's working directory.
GITEA_RUNNER_JOB_CONTAINER_FORCE_PULL|`false`|Pull docker images even if already present
GITEA_RUNNER_JOB_TIMEOUT|`3h`|The maximum time a job can run before it is cancelled
GITEA_RUNNER_ENV_VAR_**N**_NAME|`<empty>`|Name of the **N**-th extra environment variable to be passed to Job containers, e.g. `GITEA_RUNNER_ENV_VAR_1_NAME=MY_AUTH_TOKEN`
GITEA_RUNNER_ENV_VAR_**N**_VALUE|`<empty>`|Value of the **N**-th extra environment variable to be passed to Job containers, e.g. `GITEA_RUNNER_ENV_VAR_1_VALUE=SGVsbG8gbXkgZnJpZW5kIQ==`
GITEA_RUNNER_VALID_VOLUME_**N**|`<empty>`|Volumes (including bind mounts) that are allowed to be mounted into job containers. [Glob syntax](https://github.com/gobwas/glob) is supported, e.g. `GITEA_RUNNER_VALID_VOLUME_1=/src/*.json`
GITEA_RUNNER_HOST_WORKDIR_PARENT|`/data/cache/actions`|The parent directory of a job's working directory. (Path to cache cloned actions)

#### Embedded cache server:
Name|Default Value|Description
----|-------------|-----------
ACT_CACHE_SERVER_ENABLED|`true`| Enable the use of an embedded or external cache server with `actions/cache` in jobs
ACT_CACHE_SERVER_EXTERNAL_URL|`<empty>`|URL to an external cache server. If specified, act_runner will use this URL as the ACTIONS_CACHE_URL instead of starting an embedded server. The URL should end with "/".
ACT_CACHE_SERVER_DIR|`/data/cache/server`| The directory to store the cache data
ACT_CACHE_SERVER_HOST|`<empty>`| The IP address or hostname via which the job containers can reach the cache server. Leave empty for automatic detection
ACT_CACHE_SERVER_PORT|`0`|The TCP port of the cache server. `0` means to use a random, available port

## <a name="license"></a>License

All files in this repository are released under the [Apache License 2.0](LICENSE.txt).

Individual files contain the following tag instead of the full license text:
```
SPDX-License-Identifier: Apache-2.0
```

This enables machine processing of license information based on the SPDX License Identifiers that are available here: https://spdx.org/licenses/.

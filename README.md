# init-mysql

## Synopsis

Initialized a MySQL database for use with Senzing.

## Overview

The [init-mysql.py](init-mysql.py) python script...
The `senzing/init-mysql` docker image is a wrapper for use in docker formations (e.g. docker-compose, kubernetes).

To see all of the subcommands, run:

```console
$ ./init-mysql.py
usage: template-python.py [-h] {all,sleep,version,docker-acceptance-test} ...

Add description. For more information, see https://github.com/Senzing/template-python

positional arguments:
  {all,sleep,version,docker-acceptance-test}
                        Subcommands (SENZING_SUBCOMMAND):
    all                 Perform all initialization tasks.
    sleep               Do nothing but sleep. For Docker testing.
    version             Print version of program.
    docker-acceptance-test
                        For Docker acceptance testing.

optional arguments:
  -h, --help            show this help message and exit

```

### Contents

1. [Preamble](#preamble)
    1. [Legend](#legend)
1. [Related artifacts](#related-artifacts)
1. [Expectations](#expectations)
1. [Demonstrate using Command Line Interface](#demonstrate-using-command-line-interface)
    1. [Prerequisites for CLI](#prerequisites-for-cli)
    1. [Download](#download)
    1. [Environment variables for CLI](#environment-variables-for-cli)
    1. [Run command](#run-command)
1. [Demonstrate using Docker](#demonstrate-using-docker)
    1. [Prerequisites for Docker](#prerequisites-for-docker)
    1. [Docker volumes](#docker-volumes)
    1. [Database support](#database-support)
    1. [Run Docker container](#run-docker-container)
1. [Demonstrate using docker-compose](#demonstrate-using-docker-compose)
    1. [Clone repository for docker-compose](#clone-repository-for-docker-compose)
    1. [Volumes](#volumes)
    1. [Deploy docker-compose stack](#deploy-docker-compose-stack)
1. [Develop](#develop)
    1. [Prerequisites for development](#prerequisites-for-development)
    1. [Clone repository](#clone-repository)
    1. [Build Docker image](#build-docker-image)
1. [Examples](#examples)
    1. [Examples of CLI](#examples-of-cli)
    1. [Examples of Docker](#examples-of-docker)
1. [Advanced](#advanced)
    1. [Configuration](#configuration)
1. [Errors](#errors)
1. [References](#references)
1. [License](#license)

## Preamble

At [Senzing](http://senzing.com),
we strive to create GitHub documentation in a
"[don't make me think](https://github.com/Senzing/knowledge-base/blob/main/WHATIS/dont-make-me-think.md)" style.
For the most part, instructions are copy and paste.
Whenever thinking is needed, it's marked with a "thinking" icon :thinking:.
Whenever customization is needed, it's marked with a "pencil" icon :pencil2:.
If the instructions are not clear, please let us know by opening a new
[Documentation issue](https://github.com/Senzing/template-python/issues/new?template=documentation_request.md)
describing where we can improve.   Now on with the show...

### Legend

1. :thinking: - A "thinker" icon means that a little extra thinking may be required.
   Perhaps there are some choices to be made.
   Perhaps it's an optional step.
1. :pencil2: - A "pencil" icon means that the instructions may need modification before performing.
1. :warning: - A "warning" icon means that something tricky is happening, so pay attention.

## Related artifacts

1. [DockerHub](https://hub.docker.com/r/senzing/init-mysql)
1. [Helm Chart](https://github.com/Senzing/charts/tree/main/charts/senzing-init-mysql)

## Expectations

- **Space:** This repository and demonstration require 6 GB free disk space.
- **Time:** Budget 40 minutes to get the demonstration up-and-running, depending on CPU and network speeds.
- **Background knowledge:** This repository assumes a working knowledge of:
  - [Docker](https://github.com/Senzing/knowledge-base/blob/main/WHATIS/docker.md)

## Demonstrate using Command Line Interface

### Prerequisites for CLI

:thinking: The following tasks need to be complete before proceeding.
These are "one-time tasks" which may already have been completed.

1. Install system dependencies:
    1. Use `apt` based installation for Debian, Ubuntu and
       [others](https://en.wikipedia.org/wiki/List_of_Linux_distributions#Debian-based)
        1. See [apt-packages.txt](src/apt-packages.txt) for list
    1. Use `yum` based installation for Red Hat, CentOS, openSuse and
       [others](https://en.wikipedia.org/wiki/List_of_Linux_distributions#RPM-based).
        1. See [yum-packages.txt](src/yum-packages.txt) for list
1. Install Python dependencies:
    1. See [requirements.txt](requirements.txt) for list
        1. [Installation hints](https://github.com/Senzing/knowledge-base/blob/main/HOWTO/install-python-dependencies.md)
1. The following software programs need to be installed:
    1. [senzingapi](https://github.com/Senzing/knowledge-base/blob/main/HOWTO/install-senzing-api.md)
1. :thinking: **Optional:**  Some databases need additional support.
   For other databases, this step may be skipped.
    1. **Db2:** See
       [Support Db2](https://github.com/Senzing/knowledge-base/blob/main/HOWTO/support-db2.md).
    1. **MS SQL:** See
       [Support MS SQL](https://github.com/Senzing/knowledge-base/blob/main/HOWTO/support-mssql.md).
1. [Configure Senzing database](https://github.com/Senzing/knowledge-base/blob/main/HOWTO/configure-senzing-database.md)
1. [Configure Senzing](https://github.com/Senzing/knowledge-base/blob/main/HOWTO/configure-senzing.md)

### Download

1. Get a local copy of
   [template-python.py](template-python.py).
   Example:

    1. :pencil2: Specify where to download file.
       Example:

        ```console
        export SENZING_DOWNLOAD_FILE=~/init-mysql.py
        ```

    1. Download file.
       Example:

        ```console
        curl -X GET \
          --output ${SENZING_DOWNLOAD_FILE} \
          https://raw.githubusercontent.com/Senzing/init-mysql/main/init-mysql.py
        ```

    1. Make file executable.
       Example:

        ```console
        chmod +x ${SENZING_DOWNLOAD_FILE}
        ```

1. :thinking: **Alternative:** The entire git repository can be downloaded by following instructions at
   [Clone repository](#clone-repository)

### Environment variables for CLI

1. :pencil2: Identify the Senzing `g2` directory.
   Example:

    ```console
    export SENZING_G2_DIR=/opt/senzing/g2
    ```

    1. Here's a simple test to see if `SENZING_G2_DIR` is correct.
       The following command should return file contents.
       Example:

        ```console
        cat ${SENZING_G2_DIR}/g2BuildVersion.json
        ```

1. Set common environment variables
   Example:

    ```console
    export PYTHONPATH=${SENZING_G2_DIR}/python
    ```

1. :thinking: Set operating system specific environment variables.
   Choose one of the options.
    1. **Option #1:** For Debian, Ubuntu, and [others](https://en.wikipedia.org/wiki/List_of_Linux_distributions#Debian-based).
       Example:

        ```console
        export LD_LIBRARY_PATH=${SENZING_G2_DIR}/lib:${SENZING_G2_DIR}/lib/debian:$LD_LIBRARY_PATH
        ```

    1. **Option #2** For Red Hat, CentOS, openSuse and [others](https://en.wikipedia.org/wiki/List_of_Linux_distributions#RPM-based).
       Example:

        ```console
        export LD_LIBRARY_PATH=${SENZING_G2_DIR}/lib:$LD_LIBRARY_PATH
        ```

### Run command

1. Run the command.
   Example:

   ```console
   ${SENZING_DOWNLOAD_FILE} --help
   ```

1. For more examples of use, see [Examples of CLI](#examples-of-cli).

## Demonstrate using Docker

### Prerequisites for Docker

:thinking: The following tasks need to be complete before proceeding.
These are "one-time tasks" which may already have been completed.

1. The following software programs need to be installed:
    1. [docker](https://github.com/Senzing/knowledge-base/blob/main/HOWTO/install-docker.md)

### Run Docker container

1. :pencil2: Specify database connection information.
   Example:

    ```console
    export DATABASE_PROTOCOL=mysql
    export DATABASE_USERNAME=g2
    export DATABASE_PASSWORD=g2
    export DATABASE_HOST=senzing-mysql
    export DATABASE_PORT=3306
    export DATABASE_DATABASE=G2
    ```

1. Construct Database URL.
   Example:

    ```console
    export SENZING_DATABASE_URL="${DATABASE_PROTOCOL}://${DATABASE_USERNAME}:${DATABASE_PASSWORD}@${DATABASE_HOST}:${DATABASE_PORT}/${DATABASE_DATABASE}"
    ```

1. Run Docker container.
   Example:

    ```console
    sudo --preserve-env docker run \
      --env SENZING_DATABASE_URL \
      --rm \
      senzing/init-mysql mandatory
    ```

1. For more examples of use, see [Examples of Docker](#examples-of-docker).

## Demonstrate using docker-compose

1. :pencil2: Specify a new directory to hold demonstration artifacts on the local host.
   Example:

    ```console
    export SENZING_VOLUME=~/my-senzing
    ```

    1. :warning:
       **macOS** - [File sharing](https://github.com/Senzing/knowledge-base/blob/main/HOWTO/share-directories-with-docker.md#macos)
       must be enabled for `SENZING_VOLUME`.
    1. :warning:
       **Windows** - [File sharing](https://github.com/Senzing/knowledge-base/blob/main/HOWTO/share-directories-with-docker.md#windows)
       must be enabled for `SENZING_VOLUME`.

1. Set environment variables.
   Example:


    ```console
    export PGADMIN_DIR=${SENZING_VOLUME}/pgadmin
    export MYSQL_DIR=${SENZING_VOLUME}/mysql
    export SENZING_VAR_DIR=${SENZING_VOLUME}/var
    ```

1. Create directories.
   Example:

    ```console
    mkdir -p ${PGADMIN_DIR} ${POSTGRES_DIR} ${RABBITMQ_DIR} ${SENZING_VAR_DIR}
    ```

1. Get stable versions of Docker images.
   Example:

    ```console
    curl -X GET \
        --output ${SENZING_VOLUME}/docker-versions-stable.sh \
        https://raw.githubusercontent.com/Senzing/knowledge-base/main/lists/docker-versions-stable.sh
    source ${SENZING_VOLUME}/docker-versions-stable.sh
    ```

1. Download `docker-compose.yaml` and Docker images.
   Example:

    ```console
    curl -X GET \
        --output ${SENZING_VOLUME}/docker-compose.yaml \
        "https://raw.githubusercontent.com/Senzing/init-mysql/main/docker-compose.yaml"
    cd ${SENZING_VOLUME}
    sudo --preserve-env docker-compose pull
    ```

1. Bring up Senzing docker-compose stack.
   Example:

    ```console
    cd ${SENZING_VOLUME}
    sudo --preserve-env docker-compose up
    ```

1. Allow time for the components to be downloaded, start, and initialize.
    1. There will be errors in some Docker logs as they wait for dependent services to become available.
       `docker-compose` isn't the best at orchestrating Docker container dependencies.

## Develop

The following instructions are used when modifying and building the Docker image.

### Prerequisites for development

:thinking: The following tasks need to be complete before proceeding.
These are "one-time tasks" which may already have been completed.

1. The following software programs need to be installed:
    1. [git](https://github.com/Senzing/knowledge-base/blob/main/HOWTO/install-git.md)
    1. [make](https://github.com/Senzing/knowledge-base/blob/main/HOWTO/install-make.md)
    1. [docker](https://github.com/Senzing/knowledge-base/blob/main/HOWTO/install-docker.md)

### Clone repository

For more information on environment variables,
see [Environment Variables](https://github.com/Senzing/knowledge-base/blob/main/lists/environment-variables.md).

1. Set these environment variable values:

    ```console
    export GIT_ACCOUNT=senzing
    export GIT_REPOSITORY=init-mysql
    export GIT_ACCOUNT_DIR=~/${GIT_ACCOUNT}.git
    export GIT_REPOSITORY_DIR="${GIT_ACCOUNT_DIR}/${GIT_REPOSITORY}"
    ```

1. Using the environment variables values just set, follow steps in [clone-repository](https://github.com/Senzing/knowledge-base/blob/main/HOWTO/clone-repository.md) to install the Git repository.

### Build Docker image

1. **Option #1:** Using `docker` command and GitHub.

    ```console
    sudo docker build \
      --tag senzing/init-mysql \
      https://github.com/senzing/init-mysql.git#main
    ```

1. **Option #2:** Using `docker` command and local repository.

    ```console
    cd ${GIT_REPOSITORY_DIR}
    sudo docker build --tag senzing/init-mysql .
    ```

1. **Option #3:** Using `make` command.

    ```console
    cd ${GIT_REPOSITORY_DIR}
    sudo --preserve-env make docker-build
    ```

## Examples

### Examples of CLI

The following examples require initialization described in
[Demonstrate using Command Line Interface](#demonstrate-using-command-line-interface).

### Examples of Docker

The following examples require initialization described in
[Demonstrate using Docker](#demonstrate-using-docker).

1. Examples of use:
    1. [docker-compose-demo](https://github.com/Senzing/docker-compose-demo)
    1. [kubernetes-demo](https://github.com/Senzing/kubernetes-demo)

## Advanced

### Configuration

Configuration values specified by environment variable or command line parameter.

- **[SENZING_DATA_DIR](https://github.com/Senzing/knowledge-base/blob/main/lists/environment-variables.md#senzing_data_dir)**
- **[SENZING_DATABASE_URL](https://github.com/Senzing/knowledge-base/blob/main/lists/environment-variables.md#senzing_database_url)**
- **[SENZING_DEBUG](https://github.com/Senzing/knowledge-base/blob/main/lists/environment-variables.md#senzing_debug)**
- **[SENZING_ENGINE_CONFIGURATION_JSON](https://github.com/Senzing/knowledge-base/blob/main/lists/environment-variables.md#senzing_engine-configuration_json)**
- **[SENZING_ETC_DIR](https://github.com/Senzing/knowledge-base/blob/main/lists/environment-variables.md#senzing_etc_dir)**
- **[SENZING_G2_DIR](https://github.com/Senzing/knowledge-base/blob/main/lists/environment-variables.md#senzing_g2_dir)**
- **[SENZING_LOG_LEVEL](https://github.com/Senzing/knowledge-base/blob/main/lists/environment-variables.md#senzing_log_level)**
- **[SENZING_SLEEP_TIME_IN_SECONDS](https://github.com/Senzing/knowledge-base/blob/main/lists/environment-variables.md#senzing_sleep_time_in_seconds)**
- **[SENZING_SUBCOMMAND](https://github.com/Senzing/knowledge-base/blob/main/lists/environment-variables.md#senzing_subcommand)**

## Errors

1. See [docs/errors.md](docs/errors.md).

## References

## License

View
[license information](https://senzing.com/end-user-license-agreement/)
for the software container in this Docker image.
Note that this license does not permit further distribution.

This Docker image may also contain software from the
[Senzing GitHub community](https://github.com/Senzing/)
under the
[Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).

Further, as with all Docker images,
this likely also contains other software which may be under other licenses
(such as Bash, etc. from the base distribution,
along with any direct or indirect dependencies of the primary software being contained).

As for any pre-built image usage,
it is the image user's responsibility to ensure that any use of this image complies
with any relevant licenses for all software contained within.

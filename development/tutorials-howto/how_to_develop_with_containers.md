# How to Develop With Containers

Because various EDKII project can have specific build tools and environment requirements
it can be beneficial to use a container build environment for local development.
TianoCore maintains container images for various Operating Systems and tool chains in the
[TianoCore Containers repository](https://github.com/tianocore/containers). These
images contain the build tools and environment configurations to ensure a consistent
and reproducible build between local development, CI, and pipeline platform builds.

If you are new to the concept of containers, more information can be found
[on the Docker website](https://www.docker.com/resources/what-container/).

## Setting up Docker

```admonish important "Docker Licensing"
Some Docker software is not free and some uses of Docker may require a personal or
Business subscription. Users should verify their use is licensed for their organization
if necessary.
```

The existing TianoCore container images are created for use with Docker. On Linux,
docker can be installed through the distributions package management application.
For specific builds and for other operating systems, see the [Docker install
page](https://docs.docker.com/engine/install/).

### Windows Docker Setup

It is strongly recommended to use the WSL 2 based engine when running Docker on
Windows. This can be configured in the General settings in Docker Desktop. Generally,
this provides better performance and overall design over the legacy. Details on
this framework can be found in the [Docker documentation](https://docs.docker.com/desktop/windows/wsl/).

## Local Development

To use the build container images to develop locally, there are several configurations
depending the developers preferences. This section details some tools and tips that
make using containers for local development easier. This section is not comprehensive
however and it is encouraged users experiment and consider contributing back any
new useful configurations or tools to this documentation.

> NOTE:
> If the code base is cloned in Windows, it is not advised to directly open
> this repository in a Linux dev container as the file system share between Windows
> and WSL 2 causes a very significant performance reduction. Instead, clone the
> repo in the WSL file system and map into the container or directly clone into
> the container.
>
> For example:
>
> - Install the WSL extension in VS code
> - Launch a WSL 2 instance
> - Clone the repository to a directory within the instance, not a mounted Windows drive
> - `cd` into the cloned repository directory
> - Use the command `code .` to start VS Code
> - VS Code should offer you the option 'Reopen in Container', click on it

### Manually configuring the container

Some developers may wish to manually maintain their containers. This is useful
for using editors that do not have native support for containers or the specific
type of containers used.

First, select the image most appropriate from the [TianoCore containers
list](https://github.com/tianocore/containers#current-status)
or a custom image for a specific platform or project. This image can be pulled down
from the docker command line.

```bash
docker pull ghcr.io/tianocore/containers/<image>:<tag>
```

After pulling the image, the image can be started. It is useful to map in the workspace
from the host OS as this allows easy access to the code and build files from the
host as well as the container.

```bash
docker run -i -v <local-code-directory>:<container-directory> --workdir=<container directory> --name=<name> ghcr.io/tianocore/containers/<image>:<tag>
```

After the container is existed, it can be resume by using the start command.

```bash
docker start -i <name>
```

When running in the container, the build instructions should be used as they normally
would using the stuart commands.

### Visual Studio Code

The Visual Studio Code [Dev Container extension](https://code.visualstudio.com/docs/devcontainers/containers)
provides an easy and consistent way to use containers for local development. At
the time of writing, this extension only supports Linux based containers. This
extension provides a number of useful additions to the specified docker image on
creation.

- Configures git credential manager to pipe in git credentials.
- Makes extensions available on code inside the container.
- Abstracts management of the container for seamless use in the editor.

For a shared docker image configuration, this can be configured by creating a
.devcontainer file in the repository. Some useful configurations are details below.

| Configuration          | Purpose |
| :------------          | :------ |
| "privileged": true     | This may be needed for access to KVM for QEMU acceleration.  |
| `"forwardPorts": [####]` | Can be used to forward debug or serial ports to the host OS. |

It may also be desireable to run initialization commands using the "postCreateCommand"
option. Specifically running `git config --global --add safe.directory ${containerWorkspaceFolder}`
may be required if mapping the repository into the container is expected.

And example of a devcontainer used for a QEMU platform repo is included below.

```json
{
  "image": "ghcr.io/tianocore/containers/fedora-35-dev:latest",
  "postCreateCommand": "git config --global --add safe.directory ${containerWorkspaceFolder} && pip install --upgrade -r pip-requirements.txt",
  "forwardPorts": [5000],
  "privileged": true
}
```

## Pipeline Builds

Both Azure pipelines and github workflows have native support for containers. Information
on this can be found in the [Azure Pipeline
Documentation](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/container-phases?view=azure-devops)
and the [Github Workflow
Documentation](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions).

One important detail to note is that Azure pipelines will create a new user in the
docker image during the pipeline. This user is used for all tasks and operations
and so certain local user install locations may not be by default on the path.

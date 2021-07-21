Building Kubernetes
Building Kubernetes is easy if you take advantage of the containerized build environment. This document will help guide you through understanding this build process.Requirements
Docker, using one of the following configurations:

Windows with Docker Desktop WSL2 backend Install Docker according to the instructions. Be sure to store your sources in the local Linux file system, not the Windows remote mount at /mnt/c. Note: You will need to check if Docker CLI plugin buildx is properly installed.

Overview
While it is possible to build Kubernetes using a local golang installation, we have a build process that runs in a Docker container. This simplifies initial set up and provides for a very consistent build and test environment.

Key scripts
The following scripts are found in the build/ directory. Note that all scripts must be run from the Kubernetes root directory.

build/run.sh: Run a command in a build docker container. Common invocations:
build/run.sh make: Build just linux binaries in the container. Pass options and packages as necessary.
build/run.sh make cross: Build all binaries for all platforms. To build only a specific platform, add KUBE_BUILD_PLATFORMS=<os>/<arch>
build/run.sh make kubectl KUBE_BUILD_PLATFORMS=darwin/amd64: Build the specific binary for the specific platform (kubectl and darwin/amd64 respectively in this example)
build/run.sh make test: Run all unit tests
build/run.sh make test-integration: Run integration test
build/run.sh make test-cmd: Run CLI tests
build/copy-output.sh: This will copy the contents of _output/dockerized/bin from the Docker container to the local _output/dockerized/bin. It will also copy out specific file patterns that are generated as part of the build process. This is run automatically as part of build/run.sh.
build/make-clean.sh: Clean out the contents of _output, remove any locally built container images and remove the data container.
build/shell.sh: Drop into a bash shell in a build container with a snapshot of the current repo code.

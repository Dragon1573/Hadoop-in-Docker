# Hadoop in Dev Container

This repository provides a "startup template" of specific Docker Dev Container. Developers can easily create, compile and run Hadoop *MapReduce* jobs with a "pseudo-distributed" deployment of *Apache Hadoop v3.4.1*.

Just follow these steps for happy development :tada:

## Build container

1. Select "Use this template" button, a brand new project under your account based on this template.

2. Clone this repository to your local device.

3. Make sure you have **Docker** and **Visual Studio Code** already installed.
   - **Dev Container** feature should be supported by your **Docker** installation.
   - **Dev Containers** plugin should also be installed inside **Visual Studio Code**.
   > [!NOTE]
   > 
   > This is only manually verified in *Windows 11 Professional Workstation* with *Docker Desktop for Windows*, using *WSL2 backend*.

4. Open local repository with **Visual Studio Code**, it will automatically suggest you relaunch the project inside **Dev Containers**.

5. Follow its guide to build the container. You will be connected to the container soon after the success.

## Launch Hadoop

For convenience, developers will be connected to the container as user `hadoop`. The already-configured password is also `hadoop`.

> [!CAUTION]
>
> This is only a Docker **Dev Container**. It's target for temporary tests.
>
> DO NOT USE IN PRODUCTION!!!

```bash
# Start OpenSSH Server
sudo service ssh start

# Start HDFS Service
/usr/local/hadoop-3.4.1/sbin/start-dfs.sh

# Stop HDFS Service
/usr/local/hadoop-3.4.1/sbin/stop-dfs.sh
```

## Additional information

- Docker image is based on latest *Ubuntu*, currently `v24.04 Noble Numbat`.

- `apt` of this image uses [Huawei Cloud Mirror](https://mirrors.huaweicloud.com/) instead of Ubuntu official mirror.

- `Dockerfile` is provided as `.devcontainer/Dockerfile`. It might be able to use as a standalone recipe for building Docker images.

  > [!CAUTION]
  >
  > Use it DIRECTLY at your own risk! Use as **Dev Containers** (as mentioned above) is recommended.
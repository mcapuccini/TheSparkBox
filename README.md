# TheSparkBox

[![Build Status](https://travis-ci.org/mcapuccini/TheSparkBox.svg?branch=master)](https://travis-ci.org/mcapuccini/TheSparkBox)

TheSparkBox is an all-in-one Spark deployment that you can use to fire up a local cluster. Deploying TheSparkBox you will get:

- One Spark master
- Two Spark workers
- A Jupyter interface for interactive analysis

Everything is going to get deployed on your workstation, so you will be able to test your Spark applications locally. The advantege of this approach over running Spark applications in `local` mode, is that you will get a sandbox that is closer to a real production environment. 

## Table of Contents

- [Getting Started](#getting-started)
  - [Install Docker](#install-docker)
  - [Get TheSparkBox](#get-thesparkbox)
  - [Run TheSparkBox](#run-thesparkbox)
- [Where is the distributed storage?](where-is-the-distributed-storage)
- [Configuration](#configuration)

## Getting Started

### Install Docker
TheSparkBox uses [Docker](https://www.docker.com/) to fire up the environment. Plase make sure that Docker is installed in your workstation, and that your user has the rights to pull images and to start containers. You can find the Docker installation guide following this link: https://docs.docker.com/engine/installation.

### Get TheSparkBox
TheSparkBox comes as a single executable that you can install with one line:

```bash
curl -Lo tsb https://raw.githubusercontent.com/mcapuccini/TheSparkBox/master/bin/tsb && chmod +x tsb && sudo mv tsb /usr/local/bin/
```

### Run TheSparkBox

To fire up a local Spark cluster you can run:

```bash
tsb up -d 
```

> **Note:** `-d` stands for *detached mode*, and it runs all of the services in background

The first time you fire up the cluster, the process might take several minutes, as Docker needs to download all of the required images. On the next executions the cluster is going to be ready in a bunch of seconds. 

If everything went good, you should be able to reach the UIs at:

- http://localhost:8080 (Spark master)
- http://localhost:8888 (Jupyter)

When you are done with your local Spark cluster, you can tear it down as it follows:

```bash
tsb down
```

Your Jupyter notebooks, and all of the data in the Jupyter working directory is going to persist in your computer in the `~/.TheSparkBox/data` directory.

## Where is the distributed storage?
There is no need to have a distributed file system in a single-node deployment. However, you may wonder how data can be accessed concurrently by the worker nodes, the master and the Jupyter notebook. TheSparkBox uses your host operating system storage to achieve this. A host path volume is mounted on `/home/jovyan/work` on each cluster component, hence as long as you access files in that path your local Spark cluster is going to handle concurrent operation correctly. 

## Configuration
You can tune TheSparkBox setting the following environment variables:

- **TSB_DATA_DIR:** TheSparkBox data directory (default: `~/.TheSparkBox/data`)
- **SPARK_WORKER_CORES:** number of cores for each worker (default: `2`)
- **SPARK_WORKER_MEMORY:** amount of memory  for each worker (default: `1g`)

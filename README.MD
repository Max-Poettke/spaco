# Maxotar BOGY tutorial

## Overview

This project is a complete beginners tutorial for web development using HTML, CSS and JavaScript based on the material provided by w3schools.

To spice things up a bit, we containerized (docker) the basic files and also created a simple kubernetes deployment.

With this setting we finally set up a kubernetes cluster on Google Cloud Engine, including a static IP and an Ingress controller.

At this point (February 2020) the result is available at <http://maxotar.vccserver.com>

## Prerequisites

### Downloads

To continue on this, you will need at least (Development environment)

- Visual Studio Code: <https://code.visualstudio.com/download>
- Git: <https://git-scm.com/download/win>
- TortoiseGit: <https://tortoisegit.org/download/>
- NodeJs: <https://nodejs.org/en/download/>
- Docker Desktop: <https://hub.docker.com/editions/community/docker-ce-desktop-windows>
- Google Cloud SDK <https://cloud.google.com/sdk/docs/downloads-versioned-archives>

### Configs

- Activate Kubernetes Support <https://docs.docker.com/docker-for-windows/#kubernetes>
- Configure Git: <https://docs.gitlab.com/ee/gitlab-basics/start-using-git.html#add-your-git-username-and-set-your-email>
- Login to docker.hub
  - Type: ```docker login```
  - Enter user / password for docker hub
- To deploy to the internet you need access to a public kubernetes cluster

## Update instruction

Quick summary on how to continue to work on this. We also need to collect the prequisites that need to be installed.

### 1. Develop things locally

1. Run visual Studio Code.
2. Press Ctrl-Shift-P.
3. Select "Open new external Terminal"
4. In the terminal run

   ```bash
   watch-http-server
   ```

5. Open website at <http://localhost:8080>
6. Start editing files like index.html, etc.

### 2. Create and test a docker image

1. Update kubernetes/deployment.yaml

    In line 18 increase the version number of the image by one (v1 --> v2, etc.)

    ```yaml
    18        image: maxpoettke/spaco:v1
    ```

2. In the terminal window run

   ```bash
   docker build -t maxpoettke/spaco:v1 .
   ```

    Replace "v1" with a new version number from step 1.

3. Test the image locally. Run (replace v1 with new version again)

    ```bash
    docker run --name=maxotar -d -p 8085:80 maxpoettke/spaco:v1
    ```

4. Start

    Open the browser at <http://localhost:8085> to test it.

5. After test remove the local container

    ```bash
    docker stop maxotar
    docker rm maxotar
    ```

### 3. Deploy the new version to kubernetes

1. Push your image to dockerhub (as always replace "v1" with current version)

    ```bash
    docker push maxpoettke/spaco:v1
    ```

2. Execute

    ```bash
    kubectl apply -f kubernetes/deployment.yaml
    ```

3. Open public website (can take some seconds to update)

    <http://maxotar.vccserver.com>
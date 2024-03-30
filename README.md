<div align="center">
  <h3 align="center">Learn Docker</h3>

   <div align="center">
     Learn how to Dockerize various applications step by step with detailed.
    </div>
</div>

# Table of Contents

- [Introduction](#introduction)
- [Tech Stack](#tech-stack)
- [Quick Start](#quick-start)
  - [Pre-requisites](#pre-requisites)
  - [Cloning the Repository](#cloning-the-repository)
  - [Running `hello-docker`](#running-hello-docker)
  - [Running `react-docker`](#running-react-docker)
  - [Publish images to the Docker Hub](#publish-images-to-the-docker-hub)

# Introduction

Learn the process of containerizing frontend, backend, and database applications built with diverse tech stacks like React, Vue, Svelte, or any Vite projects. Additionally, it covers examples of the containerization of complete full-stack applications, including MERN setups or the popular Monorepo full-stack applications using Next.js 14+.

This repository contains the corresponding code for all these dockerized applications using the latest Docker features, including docker-compose watch and init.

# Tech Stack

- Docker
- Node.js
- React.js
- Vite
- MongoDB
- Express.js
- Next.js
- Tailwind CSS

# Quick Start

Follow these steps to set up the project locally on your machine.

## Pre-requisites

Make sure you have the following installed on your machine:

- [Git](https://git-scm.com/)
- [Node.js](https://nodejs.org/en)
- [npm](https://www.npmjs.com/) (Node Package Manager)
- [Docker](https://www.docker.com/products/docker-desktop/)
- [MongoDB Compass](https://www.mongodb.com/products/tools/compass)

## Cloning the Repository

```bash
git clone https://github.com/armandwipangestu/learn-docker
cd learn-docker
```

## Running `hello-docker`

<details>
<summary><code>Build image</code></summary>

> **Note**:
>
> - `-t` This is option/flag for tag, if empty it will be used `latest` for default
> - `hello-docker` this is for the image name
> - `.` this mean use the current `Dockerfile` configuration

```bash
cd hello-docker
docker build -t hello-docker .
```

</details>

<details>
<summary><code>List docker images</code></summary>

```bash
docker images
```

</details>

<details>
<summary><code>Running the image</code></summary>

> **Note**:
>
> This will make container to running from the image.
>
> You can add some option/flag to make interactive shell the running container. e.g.,
>
> ```bash
> docker run -it hello-docker sh
> ```

```bash
docker run hello-docker
```

</details>

## Running `react-docker`

<details>
<summary><code>Build image</code></summary>

```bash
cd react-docker
docker build -t react-docker .
```

</details>

<details>
<summary><code>Running the image</code></summary>

```bash
docker run react-docker
```

</details>

<details>
<summary><code>Running the image with expose port</code></summary>

> **Note**:
>
> Because this is React.js application using vite, so this app will run on port `5173`. If you run the docker with this command
>
> ```bash
> docker run react-docker
> ```
>
> This app will run but not be expose to the machine host, so you can't access the app directly from your browser machine host. To handle this you must add specific option/flag to expose the port. You can use `port mapping` like this
>
> `5173:5173` The first port this mean is the machine host port, and the second port is the port inside container. If you familiar with networking, this is like port forwarding. You can imagine, when you access the `localhost:5173` from the browser machine host, under the hood it will be forward to the `5173` port on the container app
>
> If you already `port mapping` the docker run but still can't access from the browser machine host. The problem can be from the `vite`, because the default vite will not be bind to the another network (the default just bind to `localhost`). So you can change the configuration vite on `package.json` with the option/flag `--host` like this
>
> ```json
> {
>    ...
>    "scripts": {
>       "dev": "vite --host",
>       ...
>    }
>    ...
> }
> ```

```bash
docker run -p 5173:5173 react-docker
```

</details>

<details>
<summary><code>Running the image with code change detected</code></summary>

> **Note**:
>
> If you run the docker with this command
>
> ```bash
> docker run -p 5173:5173 react-docker
> ```
>
> It will be running, but when you make some change to the code, it will not be affect to the code in the container. So you must be re-build the image and then run again the container (it will be take times to build and run again the container).
>
> To handle this problem you can make mount the current directory into the `/app` container directory (The `/app` is the WORKDIR of the application, you can check from the `Dockerfile`). This effect means that the local code will be linked to the container and any change locally will be immediately clearly reflected inside the running container. This example running code
>
> ```bash
> docker run -p 5173:5173 -v "$(pwd):/app" react-docker
> ```
>
> The option/flag `-v` this is stand for the `volume` that beucase we need the volume to keep track all of those changes. Volume they try to ensure that we always have data store somewhere.
>
> But before run this command, that one last option/flag need to be assign, that is another `-v` but this time for the `/app/node_modules`
>
> Why we need this? because we need a new volume for the `node_modules` directory with the container, we do this to ensure volume mount is available it container. So now if we run the container it will use existing `node_modules` from the name volume, and any change to the dependencies want require to reinstall when starting the container.
>
> This is particularly useful when developing scenarios when you frequently start and stop containers during code changes.

```bash
docker run -p 5173:5173 -v "$(pwd):/app" -v /app/node_modules react-docker
```

</details>

## Publish images to the Docker Hub

<details>
<summary><code>Login to the user</code></summary>

```bash
cd react-docker
docker login
```

</details>

<details>
<summary><code>Create new image with existing image</code></summary>

> **Note**:
>
> This command will create a new image from the existing image `react-docker` and the name is `<username>/react-docker`. You can change `<username>` with your own username docker account.
>
> The blank `tag` it will be used the default `latest`

```bash
docker tag react-docker <username>/react-docker
```

<details>
<summary><code>Publish the image</code></summary>

> **Note**:
>
> This command will be push the image local to the Docker Hub.

```bash
docker push <username>/react-docker
```

![Docker Hub](assets/docker-hub.png)

</details>

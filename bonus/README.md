# Bonus exercises

For each of these exercises you will be asked to present your efforts at the end of the day.
Feel free to pick more than one.

1. [Configure a repo in Github that deploys to DockerHub](#Configure a repo in Github that deploys to DockerHub) :rocket:
2. [Investigate Podman (or other technologies) as a replacement for Docker Desktop](#Investigate Podman (or other technologies) as a replacement for Docker Desktop) 🚶
3. [Improve the training material on multi-stage builds](#Improve the training material on multi-stage builds) 🏙
4. [Using Poetry in a container](#Using Poetry in a container) 🕊
5. [Security best practices](#Security best practices)
6. [Building for different chipsets](#Building for different chipsets)

More details below 👇

# Configure a repo in Github that deploys to DockerHub

The goal of this exercise is to build and deploy a container to Dockerhub. 
To do this follow these steps:

1. Create a Dockerhub account, make sure to retain your username and password
2. In your **personal** Github create a public repository and clone it locally
3. Add the Docker files from one of the exercises to the root of the project
4. Create the following folder structure and Github workflow
```
└── .github
  └── workflows
    └── deploy_my_docker.yaml
```
5. In Github add two secrets: `DOCKER_USERNAME` - your username (not mail address) and `DOCKER_PASSWORD` - your password
6. Add the following code to `deploy_my_docker` and update the relevant fields:
```yaml
name: Publish Docker image

on:
  release:
    types: [published]

jobs:
  push_to_registry:
    name: Push Docker image to Docker Hub
    runs-on: ubuntu-latest
    steps:
      - name: Check out the repo
        uses: actions/checkout@v3

      - name: Log in to Docker Hub
        uses: docker/login-action@f054a8b539a109f9f41c372932f1ae047eff08c9
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Extract metadata (tags, labels) for Docker
        id: meta
        uses: docker/metadata-action@98669ae865ea3cffbcbaa878cf57c20bbf1c6c38
        with:
          images: my-docker-hub-namespace/my-docker-hub-repository

      - name: Build and push Docker image
        uses: docker/build-push-action@ad44023a93711e3deb337508980b4b5e9bcdc5dc
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```
7. Figure out how to trigger your workflow (or trigger with a different mechanism like `push`)
8. Deploy to Docker Hub and get that warm deployment feeling 🔥

> Note: Instructions are intentionally vague to get you to figure things out (or ask for help 😊)

# Investigate Podman (or other technologies) as a replacement for Docker Desktop

Docker Desktop requires a license.  It would be nice to elaborate some more in the training about other technologies for creating containers.  

Investigate and experiment, outcome should:

- What did you take a look at?
- What are the pros in comparison to Docker Desktop?
- What are the cons?

# Improve the training material on multi-stage builds

The training slides are old and contain only one slide about multi-stage builds

Investigate and write out about:

- What do they offer?
- How should you use them?
- What are the advantages/disadvantages?
- How would you teach about them to someone with minimal Docker knowlege?



# Using Poetry in a container

Based on a question asked during a recent training:

- What would it look like to use Poetry in a container?
- What would that give you?
- What are the advantages/disadvantages?
- Should we recommend this to our clients?

# Security best practices

The course material is a bit vague about security around containers

- If you were to have a single page about security, what would it contain?

# Building for different chipsets

If you have an M1 and have to work with old container images you may notice
some issues, particularly in terms of slow running containers or certain
libraries not working as expected

- Formulate a paragraph to explain what the issue is
- What does this mean in terms of building containers on one chipset for environments using other chipsets, e.g. building on amd64 for arm64?
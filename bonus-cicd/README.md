# Bonus Exercise! 

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
8. Deploy to Docker Hub and do a celebration cha-cha 💃🕺🏼

> Note: Instructions are intentionally vague to get you to figure things out (or ask for help 😊)
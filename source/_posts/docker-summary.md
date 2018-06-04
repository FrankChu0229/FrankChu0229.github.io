title: Docker Summary
date: 2018-06-04 20:47:02
tags:[docker, tools]
categories:docker
description:Docker Usage Summary
---

## Docker build image and push to remote registry

- To add `Dockerfile`
- run `docker build -t image_name .` to build image
- run `docker image ls` to list all images
- run `docker login docker.XXX.com` to login the docker registry
- run `docker tag image_name username/repository:tag` to tag the image
- run `docker push username/repository:tag` to push the image to the remote repository.



## Reference

- [Docker Tutorial](https://docs.docker.com/get-started/part2/#log-in-with-your-docker-id)

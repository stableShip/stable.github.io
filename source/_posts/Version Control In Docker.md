---

title: Version Control In Docker
date: 2021-05-12 10:19:32
tags: [Docker]

---

## What

By using docker , we can easy deploy the application. but how we control the deploy version and how can we rollback to last version if any error occur?

There is the solution we explore in development.

  

## How

To control which version docker image need to deploy to prod. we need to defined the  **target 'sign' for our docker image, something like: "master", "prod".**

To rollback to the image we want. all image need to have a **'sign' for rollback , something like: "2021-01-01 10:00:00 - asdffe", "release date + commit id".**

so we need to push two image to docker repo: `master` image and `release date + commit id` image.

master image will overwrite old image all the time. the `release date + commit id` image will be saved in repo for rollback


## Code
```
TAG = date + commit id

REGISTRY=your registry

NAME=image name

PROD_TAG = master

UAT_TAG = uat


## build prod

# build image

docker build -t ${REGISTRY}/${NAME}:${TAG} .

# create image with PROD_TAG` `for`  `PROD RELEASE

docker tag ${REGISTRY}/${NAME}:${TAG} ${REGISTRY}/${NAME}:${PROD_TAG}

docker push ${REGISTRY}/${NAME}:${TAG} && docker push ${REGISTRY}/${NAME}:${PROD_TAG}

## build UAT

docker build -t ${REGISTRY}/${NAME}:${TAG} .

docker tag ${REGISTRY}/${NAME}:${TAG} ${REGISTRY}/${NAME}:${UAT_TAG}

docker push ${REGISTRY}/${NAME}:${TAG} && docker push ${REGISTRY}/${NAME}:${UAT_TAG}
```

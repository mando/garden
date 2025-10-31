---
{"dg-publish":true,"permalink":"/tech/docker-buildx-bake/","title":"`docker buildx bake`","tags":["TIL"]}
---

I've wasted so much time writing individual `build.sh`s for projects when I coulda been `docker buildx bake`ing this whole time!

https://docs.docker.com/reference/cli/docker/buildx/bake/

Basically, Bake gives you a place to define a bunch of docker build targets:

```
target "salesforce-package-info" {
    context = "."
    dockerfile = "Dockerfile"
    platforms = ["linux/arm64"]
    args = {
        SRC_ROOT = "./salesforce-package-info"
        APP_ROOT = "./salesforce-package-info/src"
    }
    tags = ["330638252201.dkr.ecr.us-east-2.amazonaws.com/salesforce-package-info:latest"]
}

target "checkin-usage" {
    context = "."
    dockerfile = "Dockerfile"
    platforms = ["linux/amd64"]
    args = {
        SRC_ROOT = "./checkin"
        APP_ROOT = "./checkin/src"
    }
    tags = ["330638252201.dkr.ecr.us-east-2.amazonaws.com/checkin-usage:latest"]
}
```

With this bake file (written in [HCL](https://github.com/hashicorp/hcl)), I can do the following (in the same directory as the bake file):

```
docker buildx bake salesforce-package-info
docker buildx bake salesforce-package-info --push
```

And docker will translate that into a command very similar to:

```
docker buildx build \
    --build-arg SRC_ROOT="./salesforce-package-info"
    --build-arg APP_ROOT="./salesforce-package-info/src"
    -t 330638252201.dkr.ecr.us-east-2.amazonaws.com/salesforce-package-info:latest \
    --platform linux/arm64 \
    -f Dockerfile \
    .
```

This is especially useful for monorepos where you might have a bunch of these docker builds sprinkled around, and you've got a bunch of little `build.sh` files that all basically look the same that take the same set of params to do the same kinds of stuff.
# Build your own Node Red Docker image with custom packages

The docker-custom directory contains files you need to build your own images.

The follow steps describe in short which steps to take to build your own images.

## 1. git clone

Clone the Node-RED Docker project from github
```shell script
git clone https://github.com/georgefridrich/node-red-docker.git
```

Change dir to docker-custom
```shell script
cd node-red-docker/docker-custom
```

## 1. **package.json**

   - Change the node-red version in package.json (from the docker-custom directory) to the version you require
   - Add optionally packages you require

## 2. **docker-make.sh**

The `docker-make.sh` is a helper script to build a custom Node-RED docker image.

Change the build arguments as needed:

   - `--build-arg ARCH=amd64` : architecture your are building for (arm32v6, arm32v7, arm64v8, amd64)
   - `--build-arg NODE_VERSION=10` : NodeJS version you like to use
   - `--build-arg NODE_RED_VERSION=${NODE_RED_VERSION}` : don't change this, ${NODE_RED_VERSION} gets populated from package.json
   - `--build-arg OS=alpine` : the linux distro to use (alpine)
   - `--build-arg BUILD_DATE="$(date +"%Y-%m-%dT%H:%M:%SZ")"` : don't change this
   - `--build-arg TAG_SUFFIX=default` : to build the default or minimal image
   - `--file Dockerfile.custom` : Dockerfile to use to build your image.
   - `--tag manila:node-red-build` : set the image name and tag

## 3. **Run docker-make.sh**

Run `docker-make.sh`

```shell script
$ ./docker-make.sh
```

This will start building your custom image and might take a while depending on the system you are running on.

When building is done you can push the custom image to our Manila repo with the following command:

```shell script
docker push 657641368736.dkr.ecr.us-east-2.amazonaws.com/manila:node-red-build
```

AWS token must be set on the docker server to push to our secure repo.  If your token expires pr you don't have a token enter the following command on your AWS CLI enabled system:

```shell script
aws ecr get-login --no-include-email
```

*If you still cannot push the image to the Manila repo please contact George Fridrich to verify access.

## 4. **Advanced Configuration**

`Dockerfile.custom` can be modified as required. To add more applications the `scripts/install_devtools.sh` can be modified as needed.

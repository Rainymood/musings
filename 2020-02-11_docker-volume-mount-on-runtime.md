# Today I learned: Docker volume is not mounted during build

From [here](https://stackoverflow.com/questions/30043872/docker-compose-node-modules-not-present-in-a-volume-after-npm-install-succeeds)

## Context

I am currently in the process of changing our developer workflow. I believe that we can make this faster by introducing hot-reloads into our project when developing locally. 

I ran into the issue where I 

My docker-compose looks something like this: 

```yaml
version: '3.3'
services:
    test:
        build:
            context: ./
            dockerfile: Dockerfile
        volumes:
            - .:/app
```

And a toned down version of my Dockerfile something like this: 

```Dockerfile
FROM ...

WORKDIR /app

RUN npm install --no-cache --production

ENTRYPOINT ["node", "index.js"]
```

On running the dockerfile I got the following error

```bash
Step 8/12 : RUN npm install --no-cache --production
 ---> Running in daf9c539a51a
npm WARN saveError ENOENT: no such file or directory, open '/app/package.json'
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN enoent ENOENT: no such file or directory, open '/app/package.json'
npm WARN app No description
npm WARN app No repository field.
npm WARN app No README data
npm WARN app No license field.
```

How?! I mount the volume right? Wrong. 

## Reason

The reason for this error is that 

**Docker volumes are mounted at runtime, not at build time**

This is elaborated on in this great stackoverflow answer [here](https://stackoverflow.com/questions/30043872/docker-compose-node-modules-not-present-in-a-volume-after-npm-install-succeeds).

>This happens because you have added your worker directory as a volume to
your docker-compose.yml, as the volume is not mounted during the build.

>When docker builds the image, the node_modules directory is created within
the worker directory, and all the dependencies are installed there. Then on
runtime the worker directory from outside docker is mounted into the docker
instance (which does not have the installed node_modules), hiding the
node_modules you just installed. You can verify this by removing the mounted
volume from your docker-compose.yml.

A workaround is also presented **data volumes**

>A workaround is to use a data volume to store all the node_modules, as data
volumes copy in the data from the built docker image before the worker
directory is mounted. 

## Summary

* Docker volumes are mounted at runtime, not at build time. 
* Can use data volumes to make a workaround, this does introduce some side-effects wrt portability.


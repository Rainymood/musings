# Saving $30.000 with Docker hot reload

## Problem

As of writing this I am working on a Node.js application deployed in
production using Docker. To ensure we work in the same environment locally we
also develop using docker. We have several docker compose files:
`docker-compose.yaml`, `docker-compose-local.yaml `, and
`docker-compose-prd.yaml`.

Whenver we make a change in local, we have to stop the docker container and
rebuild the image, and only then we can check out whether the change broke
anything. This slows down the developer workflow and this is the problem I
want to solve. 

## Solution

The solution that immediately comes to mind is a docker-compose file with
hot-reloading for the Node.js files and modules. It took a lot of
troubleshooting but I finally did it!

For this fix I created creating a new `docker-compose.hot.yaml` file and a
`Dockerfile-hot` which gets loaded in the docker-compose. The secret sauce to the hot reload is the following piece of configuration in the `yaml` file.

```yaml
volumes:
   - .:/app
   - /app/node_modules/
```

We make a bind-mount to the app directory in our container but at the same time create an anonymous volume which allows us to use the container's local `node_modules` instead of "our" local `node_modules`. As documented in [this](2020-02-11_add-volume-to-docker-exclude-subfolder.md) post.


## Impact

To estimate the impact of this fix we consider the total amount of commits. 

```bash
$ git rev-list --all --count
895
```

Estimating a conservative 10 edits per commit we get 8950 changes. It took
roughly 2 minutes to rebuild the image and deploy the whole stack on my cmoputer, in other words 17900 minutes or 298 hours. Pegging developer time at $100/hr we obtain a conservative retroactive minimum for this fix to be $30.000.

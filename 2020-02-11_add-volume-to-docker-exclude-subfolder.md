# How to add a volume to docker but exclude a subfolder

From [here](https://stackoverflow.com/questions/29181032/add-a-volume-to-docker-but-exclude-a-sub-folder)

Add this to your `docker-compose.yaml`

```yaml
volumes:
   - .:/app
   - /app/node_modules/
```

## Why does this work? 

At first sight this fix baffled me. It is explained as follows. 

* First, we map everything in the root directory `./` to `/app`
* Second, we create another mount volume `/app/node_modules` which is now an empty directory. 
* This is an empty directory even if our local `./node_modules` is not empty.

Upon reading this, I was not any wiser. I decided to read the
[documentation](https://docs.docker.com/compose/compose-file/) and found
this:

```yaml
volumes:
  # Just specify a path and let the Engine create a volume
  - /var/lib/mysql
```

In other words, how I think it works is that it creates a binding from
`/app/node_modules` to `/app/node_modules`. However, `/app/node_modules` is
empty in our local directory and hence it creates an empty directory there as
well. (UPDATE: This is wrong!)

I think my confusion comes from confusing binds and volumes. This code
creates a so-called **bind-mount** between the host and container.

```yaml
volumes:
   - .:/app
```

And this piece of code creates an **anonymous volume**.

```yaml
volumes:
   - /app/node_modules/
```

See this answer
[here](https://stackoverflow.com/questions/54313063/i-do-not-understand-the-syntax-of-docker-compose-volumes-and-services).

In the comments to this answer Kerbalman elaborates that **all three ways
create a two-way binding between host and container**.

>I may not be very clear on that: all 3 forms create a two-way binding
between the host and the container but these 2 forms do not allow you to
choose a path. An example: If you host a database instance with docker and do
not want to specify location to put the data, this is the option to use. –
kerbalman Jan 23 '19 at 9:23

Can we test or confirm this hypothesis? 

A simple `docker inspect <CONTAINER_ID>` gives

```json
"Mounts": [
    {
        "Type": "bind",
        "Source": "...",
        "Destination": "/app",
        "Mode": "rw",
        "RW": true,
        "Propagation": "rprivate"
    },
    {
        "Type": "volume",
        "Name": "c8f47d2f1b2282b40bccd01f6a9fc9c4b5f7c27adab72bbee5b0dfb624e0220a",
        "Source": "/var/lib/docker/volumes/c8f47d2f1b2282b40bccd01f6a9fc9c4b5f7c27adab72bbee5b0dfb624e0220a/_data",
        "Destination": "/app/node_modules",
        "Driver": "local",
        "Mode": "",
        "RW": true,
        "Propagation": ""
    }
```

So indeed! Our hypothesis is correct! Nice. 

So my brain still didn't grok the solution, but now it does. See this piece of documentation: 

>If you mount an empty volume into a directory in the container in which files or directories exist, these files or directories are propagated (copied) into the volume. Similarly, if you start a container and specify a volume which does not already exist, an empty volume is created for you.

In other words: 

**When we mount /app/node_modules we don't overwrite the node_modules with an empty directory, we copy all files (none) over**

That's it! 

## Summary

```yaml
volumes:
   - .:/app # creates a bind mount
   - /app/node_modules/ # creates an anonymous volume with an empty dir keeping node_modules
```
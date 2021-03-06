# Docker Instructions

> Note: The way Docker works is that a static `Dockerfile`, containing setup instructions, is compiled into a read-only object called a `Docker image`. This image is then turned into an interactive environment called a `Docker container` when you work with Docker. As a rule, these containers are freshly generated when you use them --- persistent storage is not the goal, and it's not difficult to lose your work. 

## Quick Start Guide

1. Install [Docker](https://docs.docker.com/install/), should expect to see an executable file
    - Link [for Mac](https://store.docker.com/editions/community/docker-ce-desktop-mac)
    - For [Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows). Don't click the line for Windows containers. 
    - Note: You may need to create an account on Docker's website to download 

2. Test your Docker setup
    - Can run either `docker version` or `docker run hello-world` in your terminal. 

3. Download our interactive Dockerfile at https://raw.githubusercontent.com/ubcecon/computing_and_datascience/master/docker/interactive/Dockerfile

4. In the same directory, turn that Dockerfile into an image with `docker build -t qe-julia:interactive .`. Go get coffee.

5. Once that's done, you can use a fresh container by running `docker run -p 8888:8888 qe-julia:interactive` in your terminal. This will give you some output like: 

    ```
    127.0.0.1):8888/?token=7c8f37bf32b1d7f0b633596204ee7361c1213926a6f0a44b
    ```

    Remove the `)` before `8888` and paste this into your browser to access the Docker environment. 

5. After step 4, should see a jupyter notebook opened in the preferred browser.  From here, you can open either a new Jupyter notebook (`New => Julia`) or terminal (`New => Terminal`).

6. To shut down, run `docker stop $(docker ps -aq)` or hit `Control+C` if your terminal is occupied. 

7. To restart that exact image, see the instructions below. 

## Local Files, Persistent Storage, Etc.  

A common use case is to let your Docker container exchange files with a local directory (say, a set of notebooks). For ways to do this, see: 

* Mounting to a Local Directory: This will start your Docker container with full read/write access to files in a local directory. The way to do this is to navigate to a directory in your terminal, and run: 

```
docker run -p 8888:8888 -v "$PWD":/home/jovyan qe-julia:interactive
docker run -p 8888:8888 -v C:\Users\Arnav:/home/jovyan/work jupyter/datascience-notebook (Windows)
```

* Saving Machine State: Sometimes, you'll have done a lot of work in a given container (installing packages, files, etc.), and you would prefer to simply "shut down" the container and "restart" it later. 

   To do this, take note of the container ID generated by docker before you exit. You can find the container IDs for active containers by running `docker ps` in the terminal. Then, after you quit, run `docker start -a CONTAINERID` to restart in the state you saved. 

   :warning: Quitting a Docker container is like hard-rebooting a PC. Things you saved will stick around, but things you were working on without saving will be lost. Also, since the Docker community acts as if containers are kept running while they're needed and quit otherwise, it's always possible that they'll adopt automatic garbage-collection. Bottom line, save valuable files explicitly.

* Docker Garbage Collection: If you want to delete old containers, images, etc., run `docker system prune`.

# Docker Resources 

The beauty of Docker is that it eliminates the frictions required to test new technologies (for example, our `qe-julia:interactive` image is based on [jupyter/datascience-notebook](https://hub.docker.com/r/jupyter/datascience-notebook/)). Consider exploring [Dockerhub](https://hub.docker.com). 

See Also: 

    * [Docker for Beginners](https://github.com/docker/labs/tree/master/beginner)
    * [Docker + Atom](https://github.com/dformoso/docker-atom-tutorial)

# COLLECTION OF COMMANDS TO DOCKERIZE PYTHON ENVIRONMENT: 
#### My personal list of docker (and pip) commands to quickly on-board a python container with custom packages

## Introduction:

### Juggling code and environment issues:

This is no _bs_ collection to get up & running docker python container with your custom packages on windows. 

I was trying to use some python packages (on windows10 and Android Termux ;)) for timeseries analysis & ML which were not getting installed on either.

* Windows 10 -- due to missing VC++. (Downloading gigs of VisualStudio was very painful and not feasible) 
* Andoid Termux -- due to a [platform bug](https://github.com/asciinema/asciinema/issues/271). 

It was becoming difficult to juggle code between various environments at my home vs office workstation vs android. 

So I decied to use docker containers. Also, fortunately, GNU C/C++ compilers are lot smaller than VC++. 

All this, but had to spend some time learning and building a custom python docker container than actual work. Hence I decided to compile a list of commands and make available to you.


### `dockerfile` failed me:

I decided to start with dockerfile but later ditched the idea. 
Issue with dockerfile was due to _ephemeral storage_ of the docker containers.

Here was my `dockerfile`: 
~~~
FROM python:3.7.5-slim
MAINTAINER Rajesh Swarnkar <rjs.swarnkar@gmail.com>
RUN mkdir /rswarnka
WORKDIR /app
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
~~~

If there were build failures or any other issues while installing packages the downloaded packages were removed. 
 
I decided to build images by step by step doing docker commits. I successfully installed below packages/dependencies and then commited my packages. 
Also, I had transfered the `.whl` and `.tar.gz` on local for later creating lean python containers.

~~~
pip install   Cython-0.29.14-cp37-cp37m-manylinux1_x86_64.whl      --no-index --find-links file:///rswarnka/
pip install   XlsxWriter-1.2.7-py2.py3-none-any.whl                --no-index --find-links file:///rswarnka/
pip install   beautifulsoup4-4.8.2-py3-none-any.whl                --no-index --find-links file:///rswarnka/
pip install   cycler-0.10.0-py2.py3-none-any.whl                   --no-index --find-links file:///rswarnka/
pip install   fbprophet-0.5.tar.gz                                 --no-index --find-links file:///rswarnka/
pip install   pystan-2.19.1.1-cp37-cp37m-manylinux1_x86_64.whl     --no-index --find-links file:///rswarnka/
pip install   jdcal-1.4.1-py2.py3-none-any.whl                     --no-index --find-links file:///rswarnka/
pip install   joblib-0.14.1-py2.py3-none-any.whl                   --no-index --find-links file:///rswarnka/
pip install   kiwisolver-1.1.0-cp37-cp37m-manylinux1_x86_64.whl    --no-index --find-links file:///rswarnka/
pip install   lxml-4.5.0-cp37-cp37m-manylinux1_x86_64.whl          --no-index --find-links file:///rswarnka/
pip install   matplotlib-3.1.2-cp37-cp37m-manylinux1_x86_64.whl    --no-index --find-links file:///rswarnka/
pip install   numpy-1.18.1-cp37-cp37m-manylinux1_x86_64.whl        --no-index --find-links file:///rswarnka/
pip install   openpyxl-2.0.2-py2.py3-none-any.whl                  --no-index --find-links file:///rswarnka/
pip install   pandas-1.0.0-cp37-cp37m-manylinux1_x86_64.whl        --no-index --find-links file:///rswarnka/
pip install   patsy-0.5.1-py2.py3-none-any.whl                     --no-index --find-links file:///rswarnka/
pip install   pmdarima-1.5.2-cp37-cp37m-manylinux1_x86_64.whl      --no-index --find-links file:///rswarnka/
pip install   pyparsing-2.4.6-py2.py3-none-any.whl                 --no-index --find-links file:///rswarnka/
pip install   python_dateutil-2.8.1-py2.py3-none-any.whl           --no-index --find-links file:///rswarnka/
pip install   pytz-2019.3-py2.py3-none-any.whl                     --no-index --find-links file:///rswarnka/
pip install   scikit_learn-0.22.1-cp37-cp37m-manylinux1_x86_64.whl --no-index --find-links file:///rswarnka/
pip install   scipy-1.4.1-cp37-cp37m-manylinux1_x86_64.whl         --no-index --find-links file:///rswarnka/
pip install   seaborn-0.10.0-py3-none-any.whl                      --no-index --find-links file:///rswarnka/
pip install   setuptools-45.1.0-py3-none-any.whl                   --no-index --find-links file:///rswarnka/
pip install   six-1.14.0-py2.py3-none-any.whl                      --no-index --find-links file:///rswarnka/
pip install   soupsieve-1.9.5-py2.py3-none-any.whl                 --no-index --find-links file:///rswarnka/
pip install   statsmodels-0.11.0-cp37-cp37m-manylinux1_x86_64.whl  --no-index --find-links file:///rswarnka/
pip install   xlrd-1.2.0-py2.py3-none-any.whl                      --no-index --find-links file:///rswarnka/

~~~


## Field used in commands

| Field                            | Description                                                                                | 
|----------------------------------|--------------------------------------------------------------------------------------------|
| [IMAGE_ID]                       | image id of the image use `docker images` command to check                                 |
| [IMAGE_NAME]                     | name of the running container `docker images` command to check                                 |
| [CONTAINER_ID]                   | container id of the image use `docker ps` command to check                                 |
| [CONTAINER_NAME]                 | container id of the image use `docker ps` command to check                                 |
| [TAG_NAME]                       | container id of the image use `docker ps` command to check                                 |


## Building docker image using `dockerfile`:

* Build docker image from dockerfile:
  ~~~
  docker build -t [IMAGE_NAME]:[TAG_NAME] -f Dockerfile  .
  ~~~

## Running docker container:

* Run a docker container:
> **Note on running docker in background** : docker containers exit if there is no running application in the container. Docker documentation suggests that combining `-i` and `-t` options will cause it to behave like a shell. Use either of the below commands:
  ~~~
  docker run -t -d [IMAGE_NAME]:[TAG_NAME]
  docker run -i -d [IMAGE_NAME]:[TAG_NAME]
  docker run -it -d [IMAGE_NAME]:[TAG_NAME]
  docker run --name [my_cool_container_name] -it -d [IMAGE_NAME]:[TAG_NAME]
  ~~~


* Run a docker container with mounting local folder :
  ~~~
  docker run -it -d -v c:/path/to/folder:/path/in/container [IMAGE_NAME] /bin/sh
  example: docker run -it -d -v c:/docker/docker_data:/rswarnka anumaan:v1.12
  ~~~

* Get running containers:
  ~~~ 
  docker ps
  ~~~

## SSH/Login into a docker container

* log into running container from terminal or cmd:
  ~~~ 
  docker exec -it [CONTAINER_ID] /bin/bash
  docker exec -it [CONTAINER_NAME] /bin/bash
  ~~~

 

## Copying files

* From host to container:
  ~~~ 
  docker cp c:/localpath/foo.txt [CONTAINER_NAME]:/path/in/container/foo.txt
  ~~~

* From cotainer to the host :
  ~~~ 
  docker cp [CONTAINER_NAME]:/path/in/container. .
  ~~~


## Stop running container


* Stop the running docker container:
  ~~~ 
  docker stop [CONTAINER_NAME]
  ~~~

## Clean exited containers

* Remove all exited containers: 
  > You can locate containers using docker `ps -a` and filter them by their status: created, restarting, running, paused, or exited. 
  ~~~
  docker ps -a -f status=exited
  docker rm -v (docker ps -a -f status=exited -q) # linux 
  docker rm -v [CONTAINER_NAME] [CONTAINER_NAME] [CONTAINER_NAME] # windows, list of container names
  ~~~



## Setting up Python packages in container

### Checking Python paths

* Multiple pythons in linux container:
  > It is always good to check container's Python 2.x env paths vs custom Python's path
  > All the packages must be 3.7 and not 2.7
  ~~~
  python -c 'import sys; print(sys.path)'
  
  ['', '/usr/local/lib/python37.zip', '/usr/local/lib/python3.7', '/usr/local/lib/python3.7/lib-dynload', '/usr/local/lib/python3.7/site-packages'] 
  ~~~

### Downloading python packages

While building custom python image with custom packages in `dockerfile` you might loose the archives on from _ephemeral storage_  in containers.
Copy packages to the local mapped volume so that you don't loose in case of any install error occurs.

* Download python packages from requirements.txt to folder
  ~~~
  pip wheel -r requirements.txt -w .\outputdir
  ~~~  

* Download binary python packages to local dir
> If you need fast install from local archives, without probing PyPI use the below commands to first download the binary only archives:
  ~~~
  pip download --only-binary :all: --dest /path/in/container python-pypi-packagename
  >> example: pip download --only-binary :all: --dest /rswarnka/ pmdarima
  >> example: pip download --dest /rswarnka/ fbprophet
  ~~~  
> Then copy packages to the local host so that you don't loose them
  ~~~
  docker cp [CONTAINER_NAME]:/path/in/container/. .
  >> example: docker cp trusting_brown:/rswarnka/. .
  ~~~

### Installing python packages from `.whl` or `tar.tg`

* Installing packages from local dir into container python environment:
  ~~~
  > First copy the downloaded wheel packages into the running container using docker cp command outside container:
  docker cp my-wheel-pkg.whl [CONTAINER_NAME]:/path/inside/container
  > Then install the package using `--find-links`:
  pip install wheel-package.whl --no-index --find-links file:///path/to/folder
  >> example: pip install wheel-package.whl --no-index --find-links file:///rswarnka/
  ~~~

## Committing changes

* Commit changes to a docker image after modification into docker container

  ~~~
  > When you commit to changes, you essentially create a new image with an additional layer that modifies the base image layer
  sudo docker commit [CONTAINER_ID] [NEW_IMAGE_NAME]:[TAG_NAME]
  >> example: docker commit b7478fe049cb anumaan:v1.10
  ~~~


## Using container to run Jupyter Notebook
* Run the jupyter notebook with port bindings

  ~~~
  docker run -it --rm -p 8888:8888 anumaan:v1.16 jupyter notebook --ip=0.0.0.0 --port=8888 --allow-root
  docker run -it -d --rm -p 8888:8888 -v c:/docker/docker_data/:/rswarnka/  anumaan:v1.16 jupyter notebook --ip=0.0.0.0 --port=8888 --allow-root
  > Access using browser on localhost : http://127.0.0.1:8888/tree/rswarnka
  ~~~

## Future work:
* making lean python container lean
* automating using bash or powershell + docker-compose
* build fbprophet source on separate container and import build binary 


## References:

* [Install py packages using requirements.txt](https://stackoverflow.com/questions/7225900/how-to-install-packages-using-pip-according-to-the-requirements-txt-file-from-a)
* [Docker container copying files between different system](https://medium.com/@gchudnov/copying-data-between-docker-containers-26890935da3f)
* [pip Install Trouble](http://stackoverflow.com/questions/28167987/python-pip-trouble-installing-from-requirements-txt)
* [pip download command reference](https://pip.pypa.io/en/stable/reference/pip_download/)
* [Docker Issue with Running in detach mode](http://kimh.github.io/blog/en/docker/gotchas-in-writing-dockerfile-en/#hack_to_run_container_in_the_background)



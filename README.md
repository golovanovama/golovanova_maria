Dependencies management
[0.5] What is Docker, and how it differs from dependencies management systems? From virtual machines? 
Docker is an open platform for developing, shipping, and running applications. Docker provides the ability to package and run an application in a loosely isolated environment called a container.Containers are great for continuous integration and continuous delivery (CI/CD) workflows. Docker containers can run on a developer’s local laptop, on physical or virtual machines in a data center, on cloud providers, or in a mixture of environments. Docker’s portability and lightweight nature also make it easy to dynamically manage workloads, scaling up or tearing down applications and services as business needs dictate, in near real time.

[0.5] What are the advantages and disadvantages of using containers over other approaches? 
you can significantly reduce the delay between writing code and running it in production. The isolation and security allows you to run many containers simultaneously on a given host. Containers are lightweight and contain everything needed to run the application, so you do not need to rely on what is currently installed on the host. You can easily share containers while you work, and be sure that everyone you share with gets the same container that works in the same way. Docker is perfect for high density environments and for small and medium deployments where you need to do more with fewer resources.

[0.5] Explain how Docker works: what are Dockerfiles, how are containers created, and how are they run and destroyed? 
Docker uses a client-server architecture. The Docker client talks to the Docker daemon, which does the heavy lifting of building, running, and distributing your Docker containers. The Docker client and daemon can run on the same system, or you can connect a Docker client to a remote Docker daemon. The Docker client and daemon communicate using a REST API, over UNIX sockets or a network interface. Another Docker client is Docker Compose, that lets you work with applications consisting of a set of containers. Run the container - sudo docker run -it sample-image bash. To delete a container, use the rm command with the container ID or name, which can be obtained using the docker ps -a command.

[0.25] Name and describe at least one Docker competitor (i.e., a tool based on the same containerization technology).
RunC, formerly a module embedded into the Docker architecture, was released in 2015 as a standalone tool. It has since then become a widely used, standardized, interoperable container runtime DevOps teams can use as part of Docker or other custom container engines. RunC belongs to the container runtime section of the containerization ecosystem. A container runtime is a lower-level component used in a container engine that handles the running of containers. 
[0.25] What is conda? How it differs from apt, yarn, and others? Conda is a multi-platform open-source package management system. It was initially created to solve package management problems for Python data scientists.Because conda arose from within the Python (more specifically PyData) community, many mistakenly assume that it is fundamentally a Python package manager. This is not the case: conda is designed to manage packages and dependencies within any software stack. In this sense, it's less like pip, and more like a cross-platform version of apt or yum. If you want to flexibly manage a multi-language software stack and don't mind using an isolated environment, use conda. Conda's multi-language dependency management and cross-platform binary installations can do things in this situation that pip cannot do. A huge benefit is that for most packages, the result will be immediately compatible with multiple operating systems.

Problem
```
#installing python
sudo apt install python3

#installing docker
sudo apt install docker.io

#get anaconda
sudo wget https://www.anaconda.com/products/distribution

#To run it, you need to set the necessary access rights
chmod a+x ./Anaconda3-2022.10-Linux-x86_64.sh

#waiting for installing
#create virtual environment in anaconda
conda create -n bioenv

#activation 
conda activate bioenv

#Adding channels to receive additional packages specified in the task
conda config --env --add channels conda-forge
conda config --env --add channels bioconda

#installing packages
conda install -c bioconda fastqc==0.11.9 conda install -c bioconda samtools==1.16.1 conda install -c bioconda star conda install -c bioconda picard==2.27.5 conda install -c bioconda picard==2.27.4 conda install -c bioconda salmon==1.9.0 conda install -c bioconda bedtools==2.30.0

#multiqc==1.13 couldn't get this package, maybe it could be installed by hand
picard==2.27.5 - installed another version

#exporting to (bioenv.yml)
conda env export > bioenv.yml
#or
conda env export --from-history > bioenv-short.yml

#to delete
conda remove -n bioenv --all

#make the environment from file
conda env create -f bioenv.yml
#not fast

#to show existing environments
conda env list
```
Docker
Устанавливала с оф.сайта по документации https://docs.docker.com/engine/install/ubuntu/
Создала dockerfile, создала образ и запустила его, добавила автора, версию и описание.

Dockerfile is done:
```
FROM ubuntu:22.04

LABEL author="Golovanova_Maria"
LABEL version="1.0.0"
LABEL description="Dockerfile"

# we install everything that is necessary for the packages
RUN apt-get update
RUN apt-get -y --no-install-recommends install wget unzip  perl  openjdk-11-jdk xvfb python3-pip libgomp1 libtbb12

RUN touch /.bashrc

# installing packages
# FastQC==0.11.9
RUN wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip
RUN unzip fastqc_v0.11.9.zip
RUN chmod a+x /FastQC/fastqc
RUN echo 'alias fastqc="/FastQC/fastqc"' >> /.bashrc

# STAR==2.7.10b
RUN wget https://github.com/alexdobin/STAR/releases/download/2.7.10b/STAR_2.7.10b.zip
RUN unzip ./STAR_2.7.10b.zip
RUN chmod a+x ./STAR_2.7.10b/Linux_x86_64_static/STAR
RUN mv ./STAR_2.7.10b/Linux_x86_64_static/STAR /bin/STAR
RUN rm -r ./STAR_2.7.10b

# samtools==1.16.1
RUN wget https://github.com/samtools/samtools/archive/refs/tags/1.16.1.zip -O ./samtools-1.16.1.zip
RUN unzip ./samtools-1.16.1.zip
RUN mv ./samtools-1.16.1/misc /samtools
RUN rm -r ./samtools-1.16.1
RUN echo 'alias samtools="/samtools/samtools.pl"' >> /.bashrc

# picard==2.27.5
RUN wget https://github.com/broadinstitute/picard/releases/download/2.27.5/picard.jar -O /bin/picard.jar
RUN chmod a+x /bin/picard.jar
RUN echo 'alias picard="java -jar /bin/picard.jar"' >> /.bashrc

# bedtools==2.30.0
RUN wget https://github.com/arq5x/bedtools2/releases/download/v2.30.0/bedtools.static.binary -O /bin/bedtools.static.binary
RUN  chmod a+x /bin/bedtools.static.binary
RUN  echo 'alias bedtools="/bin/bedtools.static.binary"' >> /.bashrc

# salmon==1.9.0
RUN wget https://github.com/COMBINE-lab/salmon/releases/download/v1.9.0/salmon-1.9.0_linux_x86_64.tar.gz
RUN  tar -zxvf ./salmon-1.9.0_linux_x86_64.tar.gz
RUN  chmod a+x ./salmon-1.9.0_linux_x86_64/bin/salmon
RUN  mv ./salmon-1.9.0_linux_x86_64/bin/salmon /bin/salmon
RUN  rm -r ./salmon-1.9.0_linux_x86_64 

# Deleting initial sources
RUN rm ./*zip ./*tar.gz

# Clean unnecessary packages
RUN apt autoremove
RUN apt clean
```
Через анаконду образ:
```
FROM continuumio/anaconda3

# information
LABEL maintainer="Name Surname"
LABEL version="0.1"
LABEL description="RNA-seq analysis pipeline"

RUN conda create -n bioenv

# add channels
RUN conda config --env --add channels conda-forge
RUN conda config --env --add channels bioconda

# install packages
RUN conda install -n bioenv fastqc STAR samtools picard salmon bedtools 

RUN conda clean -a
```

# Working with remote servers
# [0.4] What are computer ports on a high level? How many ports are there on a typical computer?
A computer port is a connection point or interface between a computer and an external or internal device. There are many different types of computer ports that allow users to expand the functionality of their computers and connect to a range of auxiliary devices.The amount of ports on a computer depends on the model and brand of the PC. Sometimes, the port options are very minimal. In that case, you can always invest in a docking station with the number of ports and types of ports you need for your work or entertainment setup. So ports are software-based and controlled by the computer's operating system. Some of these numbers have a special purpose and are associated with certain types of processes or services, for example, port 21 is associated with the transfer of FTP files.
# [0.4] What is the difference between http, https, ssh, and other protocols? In what sense are they similar? Name default ports for several data transfer protocols.
Both ssh and HTTP are protocols to communicate between client and server.Following are the basic difference between SSH and HTTP. SSH means “Secure Shell”. It has a built-in username/password authentication system to establish a connection. It uses Port 22 to perform the negotiation or authentication process for connection. Authentication of the remote system is done by the use of public-key cryptography and if necessary, it allows the remote computer to authenticate users. HTTP means HyperText Transfer Protocol. HTTP is the underlying protocol used by the World Wide Web and this protocol defines how messages are formatted and transmitted, and what actions Web servers and browsers should take in response to various commands. However, ssh and HTTPS are secure protocols. As a result, many git servers, such as Github, Bitbuckets, and GitLab, use those two popular cryptographic network protocols. HTTPS uses port number 443 by default, and uses TCP as the transport protocol. FTP or File Transfer Protocol and TFTP or Trivial File Transfer Protocol are both optimized for downloading and uploading data. FTP uses TCP as its transport protocol on port 20 for data transfer and on port 21 for control (commands). TFTP uses UDP on port 69.
# [0.4] Explain briefly: (1) what is IP, (2) what IPs are called 'white'/public, (3) and what happens when you enter 'google.com' into the web browser.
IP (Internet Protocol) Addresses, a set of 4 numbers that range from 0 to 255 (one byte) separated by periods (ie. 127.0.0.1).
(2)IP addresses that belong to global networks are public - they are visible to other devices in the network, so they are called external, or "white". Internal IPs, on the other hand, are only used within a local network. They are also called private, or "gray".
(3)So the first thing that happens is that your browser looks up in its cache to see if that website was visited before and the IP address is known. 
If it can’t find the IP address for the URL requested then it asks your operating system to locate the website. The first place your operating system is going to check for the address of the URL you specified is in the host file. If the URL is not found inside this file, then the OS will make a DNS request to find the IP Address of the web page.
The first step is to ask the Resolver (or Internet Service Provider) server to look up its cache to see if it knows the IP Address, if the Resolver does not know then it asks the root server to ask the .COM TLD (Top Level Domain) server – if your URL ends in .net then the TLD server would be .NET and so on – the TLD server will again check in its cache to see if the requested IP Address is there. 
After the OS has the IP Address and gives it to the browser, it then makes a GET (a type of HTTP Method) to said IP Address. When the request is made the browser again makes the request to the OS which then, in turn, packs the request in the TCP traffic protocol we discussed earlier, and it is sent to the IP Address. 
On its way, it is checked by both the OS’ and the server’s firewall to make sure that there are no security violations. And upon receiving the request the server (usually a load balancer that directs traffic to all available servers for that website) sends a response with the IP Address of the chosen server along with the SSL (Secure Sockets Layer) certificate to initiate a secure session (HTTPS). 
Finally, the chosen server then sends the HTML, CSS, and Javascript files (If any) back to the OS who in turn gives it to the browser to interpret it. And then you get your website as you know it.
# [0.4] What is Nginx? How does it work on the high level? List several alternative web servers.
NGINX is a widely used open-source web server software. It was created to solve the problem of C10k, which is defined as a challenge to manage the ten thousand connections all at the same time.It can be scaled efficiently as a web server as well as a reverse proxy. It does not allow you to allocate a process to a particular connection, but it creates a process pool that can be easily shared among multiple connections within the network. Whenever a request is made, a resource will be allocated to the process resulting in better resource utilization that can easily handle extensive connections. NGINX also helps in setting up a secured connection between your data-centers and the outside network. It also works well as an HTTP load balancer that allows you to use multiple different load-sharing mechanisms.
several alternative web servers - Caddy, Apache HTTP Server, lighttpd.
# [0.4] What is SSH, and for what is it typically used? Explain two ways to authenticate in an SSH server in detail.
SSH, also known as Secure Shell or Secure Socket Shell, is a network protocol that gives users, particularly system administrators, a secure way to access a computer over an unsecured network.SSH enables the same functions -- logging in to and running terminal sessions on remote systems. SSH also replaces file transfer programs, such as File Transfer Protocol (FTP) and rcp (remote copy).The most basic use of SSH is to connect to a remote host for a terminal session. In addition to creating a secure channel between local and remote computers, SSH is used to manage routers, server hardware, virtualization platforms, operating systems (OSes), and inside systems management and file transfer applications. Secure Shell is used to connect to servers, make changes, perform uploads and exit, either using tools or directly through the terminal. SSH keys can be employed to automate access to servers and often are used in scripts, backup systems and configuration management tools.
## Problem 
```
#Created a new virtual machine in the Yandex cloud. Generated SSH key pair and used it
#I have Windows on my computer, so i generated SSH key there

ssh-keygen -t rsa -b 2048

#then i created my VM and called it - golovanova3
#then it started working

C:\Users\79202>ssh golovanova3@62.84.118.93
golovanova3@golovanova3: ~$ sudo apt update

#getting nessesary files

sudo wget https://ftp.ensembl.org/pub/release-108/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
sudo wget https://ftp.ensembl.org/pub/release-108/gff3/homo_sapiens/Homo_sapiens.GRCh38.108.gff3.gz

#then i installed nginx

sudo apt install nginx
cd/var
ls

#checked if the nessesary directory exists and continued the work

cd www
cd html
sudo mkdir jbrowse
cd /mnt
sudo mkdir JBrowse
cd JBrowse
sudo wget https://jbrowse.org/jb2/

#then i need to install program to unzip files

sudo apt install unzip
/mnt/JBrowse$ sudo unzip jbrowse-web-v2.3.1.zip
cd /etc/nginx/
/etc/nginx$ sudo nano nginx.conf
/etc/nginx$ sudo /etc/init.d/nginx reload
/etc/nginx$ cd mnt
/mnt$ cd JBrowse
/mnt/JBrowse$ cp jbrowse-web-v2.3.1.zip /var/www/html/jbrowse
/mnt/JBrowse$ cd /var/www/html/jbrowse
/var/www/html/jbrowse$ sudo unzip jbrowse-web-v2.3.1.zip
sudo gunzip Homo_sapiens.GRCh38.108.gff3.gz

#i dont know is it nessesary to write the whole path, but i think it is clear
#tried to install bgzip but got a mistake
sudo apt install bgzip

#tried this method
sudo apt install htslib
sudo apt update
sudo apt install tabix
sudo apt install pip
sudo pip install bgzip

#Sort, bgzip, and index using tabix
gunzip Homo_sapiens.GRCh38.108.gff3.gz
awk '$1 ~ /^#/ {print $0;next} {print $0 | "sort -t\"\t\" -k1,1 -k4,4n"}' Homo_sapiens.GRCh38.108.gff3 > homo-sapiens.sorted.gff3
bgzip homo-sapiens.sorted.gff3
tabix homo-sapiens.sorted.gff3.gz

#adiing to the jbrowse
sudo jbrowse add-track homo-sapiens.sorted.gff3.gz --load copy --out   /mnt/JBrowse/config.json

#it is written that it's added but i didn't get the result immidietly, but now everything is okey i guess
#then samtools
sudo apt install samtools
samtools faidx 
samtools faidx Homo_sapiens.GRCh38.dna.primary_assembly.fa
sudo jbrowse add-track homo-sapiens.sorted.gff3.gz --load copy 

#got the error
sudo apt install npm
sudo npm install -g @jbrowse/cli
jbrowse --version
sudo jbrowse add-track Homo_sapiens.GRCh38.dna.primary_assembly.fa --load copy --out /var/www/html/jbrowse

#added assembly to .... /jbrowse/config.json
```
У меня не сразу получалось добавить треки на браузер, видимо нужно было время, но теперь там несколько моих попыток понять, что происходит))
Ссылка на браузер : ![JBrowse](http://62.84.118.93/jbrowse/?session=local-JG2Pu6WXo)

**[0.5] Give an in-depth explanation of the OSI model and how the TCP/IP stack works. Don't copy-paste descriptions from the internet; paraphrase and shorten as much as possible (imagine writing a cheat sheet for yourself)**
Open System Intercommunication (OSI) это проект в области сетевых технологий.Модель OSI устанавливает глобальный стандарт, определяющий состав функциональных уровней при открытом взаимодействии между компьютерами. У моделей OSI и TCP имеется много общих черт. Обе модели основаны на концепции стека независимых протоколов. Функциональность уровней также во многом схожа. Например, в каждой модели уровни, начиная с транспортного и выше, предоставляют сквозную, не зависящую от сети транспортную службу для процессов, желающих обмениваться информацией. Стек протоколов TCP/IP — набор сетевых протоколов, на которых базируется интернет. Обычно в стеке TCP/IP верхние 3 уровня (прикладной, представительный и сеансовый) модели OSI объединяют в один — прикладной. В отличие от эталонной модели OSI, модель ТСР/IP в большей степени ориентируется на обеспечение сетевых взаимодействий, нежели на жесткое разделение функциональных уровней. Для этой цели она признает важность иерархической структуры функций, но предоставляет проектировщикам протоколов достаточную гибкость в реализации. Соответственно, эталонная модель OSI гораздо лучше подходит для объяснения механики межкомпьютерных взаимодействий, но протокол TCP/IP стал основным межсетевым протоколом.





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

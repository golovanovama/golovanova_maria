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
#_Created a new virtual machine in the Yandex cloud. Generated SSH key pair and used it_
#_I have Windows on my computer, so i generated SSH key there_

ssh-keygen -t rsa -b 2048

#_then i created my VM and called it - golovanova3_
#_then it started working_

C:\Users\79202>ssh golovanova3@62.84.118.93
golovanova3@golovanova3: ~$ sudo apt update

#_getting nessesary files_

sudo wget https://ftp.ensembl.org/pub/release-108/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
sudo wget https://ftp.ensembl.org/pub/release-108/gff3/homo_sapiens/Homo_sapiens.GRCh38.108.gff3.gz

#_then i installed nginx_
```
sudo apt install nginx
cd/var
ls

#_checked if the nessesary directory exists and continued the work_

cd www
cd html
sudo mkdir jbrowse
cd /mnt
sudo mkdir JBrowse
cd JBrowse
sudo wget https://jbrowse.org/jb2/

#_then i need to install program to unzip files_

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

#_i dont know is it nessesary to write the whole path, but i think it is clear_
#_tried to install bgzip but got a mistake_
sudo apt install bgzip

#_tried this method_
sudo apt install htslib
sudo apt update
sudo apt install tabix
sudo apt install pip
sudo pip install bgzip

#_Sort, bgzip, and index using tabix_
gunzip Homo_sapiens.GRCh38.108.gff3.gz
awk '$1 ~ /^#/ {print $0;next} {print $0 | "sort -t\"\t\" -k1,1 -k4,4n"}' Homo_sapiens.GRCh38.108.gff3 > homo-sapiens.sorted.gff3
bgzip homo-sapiens.sorted.gff3
tabix homo-sapiens.sorted.gff3.gz

#_adiing to the jbrowse_
sudo jbrowse add-track homo-sapiens.sorted.gff3.gz --load copy --out   /mnt/JBrowse/config.json

#_it is written that it's added but i didn't get the result immidietly, but now everything is okey i guess_
#_then samtools_
sudo apt install samtools
samtools faidx 
samtools faidx Homo_sapiens.GRCh38.dna.primary_assembly.fa
sudo jbrowse add-track homo-sapiens.sorted.gff3.gz --load copy 

#_got the error_
sudo apt install npm
sudo npm install -g @jbrowse/cli
jbrowse --version
sudo jbrowse add-track Homo_sapiens.GRCh38.dna.primary_assembly.fa --load copy --out /var/www/html/jbrowse

#_added assembly to .... /jbrowse/config.json
```

Ссылка на браузер : ![JBrowse](http://62.84.118.93/jbrowse/?session=local-JG2Pu6WXo)

**[0.5] Give an in-depth explanation of the OSI model and how the TCP/IP stack works. Don't copy-paste descriptions from the internet; paraphrase and shorten as much as possible (imagine writing a cheat sheet for yourself)**
Open System Intercommunication (OSI) это проект в области сетевых технологий.Модель OSI устанавливает глобальный стандарт, определяющий состав функциональных уровней при открытом взаимодействии между компьютерами. У моделей OSI и TCP имеется много общих черт. Обе модели основаны на концепции стека независимых протоколов. Функциональность уровней также во многом схожа. Например, в каждой модели уровни, начиная с транспортного и выше, предоставляют сквозную, не зависящую от сети транспортную службу для процессов, желающих обмениваться информацией. Стек протоколов TCP/IP — набор сетевых протоколов, на которых базируется интернет. Обычно в стеке TCP/IP верхние 3 уровня (прикладной, представительный и сеансовый) модели OSI объединяют в один — прикладной. В отличие от эталонной модели OSI, модель ТСР/IP в большей степени ориентируется на обеспечение сетевых взаимодействий, нежели на жесткое разделение функциональных уровней. Для этой цели она признает важность иерархической структуры функций, но предоставляет проектировщикам протоколов достаточную гибкость в реализации. Соответственно, эталонная модель OSI гораздо лучше подходит для объяснения механики межкомпьютерных взаимодействий, но протокол TCP/IP стал основным межсетевым протоколом.

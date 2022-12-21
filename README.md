# Working with remote servers
# [0.4] What are computer ports on a high level? How many ports are there on a typical computer?
A computer port is a connection point or interface between a computer and an external or internal device. There are many different types of computer ports that allow users to expand the functionality of their computers and connect to a range of auxiliary devices.The amount of ports on a computer depends on the model and brand of the PC. Sometimes, the port options are very minimal. In that case, you can always invest in a docking station with the number of ports and types of ports you need for your work or entertainment setup.
# [0.4] What is the difference between http, https, ssh, and other protocols? In what sense are they similar? Name default ports for several data transfer protocols.
# [0.4] Explain briefly: (1) what is IP, (2) what IPs are called 'white'/public, (3) and what happens when you enter 'google.com' into the web browser.
# [0.4] What is Nginx? How does it work on the high level? List several alternative web servers.
# [0.4] What is SSH, and for what is it typically used? Explain two ways to authenticate in an SSH server in detail.
## Problem 
_Created a new virtual machine in the Yandex cloud. Generated SSH key pair and used it_
_I have Windows on my computer, so i generated SSH key there_
```
ssh-keygen -t rsa -b 2048
```
_then i created my VM and called it - golovanova3_
_then it started working_
```
C:\Users\79202>ssh golovanova3@62.84.118.93
golovanova3@golovanova3: ~$ sudo apt update
```
_i guess it's okey to write only used codes without golovanova3@golovanova3_
_getting nessesary files_
```
sudo wget https://ftp.ensembl.org/pub/release-108/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
sudo wget https://ftp.ensembl.org/pub/release-108/gff3/homo_sapiens/Homo_sapiens.GRCh38.108.gff3.gz
```
_then i installed nginx_
```
sudo apt install nginx
cd/var
ls
```
_check if the nessesary directory exists and continue the work_
```
cd www
cd html
sudo mkdir jbrowse
cd /mnt
sudo mkdir JBrowse
cd JBrowse
sudo wget https://jbrowse.org/jb2/

```
_Downloaded the latest human genome assembly (GRCh38) from the Ensemble FTP server (fasta, GFF3). Index the fasta using samtools (samtools faidx) and GFF3 using tabix.
```
ssh-keygen -t rsa -b 2048
```

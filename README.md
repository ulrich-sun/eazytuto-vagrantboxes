Eazytuto
In our case, we would need to generate a virtual machine with Vagrant and VirtualBox, then install all the tools we will need later.

```bash
sudo sed -i s/mirror.centos.org/vault.centos.org/g /etc/yum.repos.d/*.repo
sudo sed -i s/^#.*baseurl=http/baseurl=http/g /etc/yum.repos.d/*.repo
sudo sed -i s/^mirrorlist=http/#mirrorlist=http/g /etc/yum.repos.d/*.repo
sudo yum -y update

# install docker
sudo yum install -y git
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
sudo usermod -aG docker vagrant
sudo systemctl enable docker
sudo systemctl start docker
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
Once this is done, we will use the Vagrant commands to create a box:
```bash
vagrant package --output name_of_the_box.box
```
Create an account on Vagrant Cloud, choose the virtualization tool, choose the work architecture, upload the file you generated previously, and release the new Vagrant box.

Once this is done, we will test our Vagrant box. To do this, we will write a Vagrantfile, then run vagrant up, connect to the VM with vagrant ssh, check the Docker service with sudo service docker status, and launch a Docker Nginx container with 
```bash docker run -dp 80:80 nginx```
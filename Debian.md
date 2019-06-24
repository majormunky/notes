# Debian

## Post Setup
Need to install sudo
Need to add user to sudoers file
    usermod -aG sudo username
    apt install sudo
    reboot

## Nginx Setup

### Install
sudo apt install nginx

## Python 3.7 Setup

### Install Libraries
sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev 
sudo apt install libssl-dev libreadline-dev libffi-dev wget curl libsqlite3-dev

### Download Source
curl -O https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tar.xz

### Extract Source
tar -xf Python-3.7.3.tar.xz

### Configure
cd Python-3.7.3
./configure

Can do ./configure --enable-optimizations, but, its really slow

### Fix SSL
cd Modules
nano Setup

Find the SSL lines and uncomment them out
Save file
cd ..

### Final Steps
make
sudo make install

## Network Setup
### Static IP
Edit /etc/network/interfaces
change dhcp to static
add lines below
    address x.x.x.x
    netmask x.x.x.x
    gateway x.x.x.x



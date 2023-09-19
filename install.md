# Registrating Your Service Node, Sispopnet and Running the Sispopd

* You have to purchase your VPS or get free tier from oracle and then update and upgrade it

```java
sudo apt update -y &&
sudo apt upgrade -y
```

* Open Ports

```java
sudo ufw allow 22020 &&

sudo ufw allow 22021 &&

sudo ufw allow 20000 &&

sudo ufw allow 50000 &&

sudo ufw allow 1090 

```

* Create a screen for sispopd

```java
screen -dmS sispop
```

```java
screen -r sispop
```
* Download the sispopd wallet

```
wget https://github.com/sispop-dev/sispop/releases/download/v10.0.0/sispop-ubuntu-wallet.tar.gz
```

* Tar the file

```java
tar -xzf sispop-wallet-ubuntu.tar.gz
```

* Go into the wallet folder

```java
cd sispop-wallet-ubuntu
```

* Run the sispopd daemon as below

```java
./sispopd --service-node --service-node-public-ip IP.ADDRESS.PUBLIC.HERE --storage-server-port 22020
```

* Let sync with blockchain and once its sync do the below
  
```java
prepare_registration
```

* Then follow commands to set up node get registration code and input into registration part of your wallet.


# Register The Sispopnet
* Open a screen for sispopnet

```java
screen -dmS sispopnet &&

screen -r sispopnet
```

* Download the Sispopnet

```java
wget https://github.com/sispop-dev/sispopnet/releases/download/v9.0.0/sispopnet-r.tar.gz &&

tar -xvf sispopnet-r.tar.gz &&

cd sispopnet-r
```

* Grant Permission to the Sispopnet 

```
sudo setcap all=+eip sispopnet
```

* Make the .sispopnet file

```
cd ~/.sispopnet
```
* Download the Bootstrap sispopnet file

```
wget https://seed.sispop.site/bootstrap.signed
```
* Grant Permission

```
chmod 775 bootstrap.signed
```

* Remove the sispopnet.ini file if it exist

```java
rm sispopnet.ini
```

* Open a new sispopnet.ini

```java
nano sispopnet.ini
```

* Then paste in below info changing username to your user
```java
[router]
threads=4
contact-file=/home/ubuntu/.sispopnet/self.signed
transport-privkey=/home/ubuntu/.sispopnet/transport.private
ident-privkey=/home/ubuntu/.sispopnet/identity.private
encryption-privkey=/home/ubuntu/.sispopnet/encryption.private
[logging]
level=info
[metrics]
json-metrics-path=/home/ubuntu/.sispopnet/metrics.json
[system]
user=sispopnet
group=sispopnet
pidfile=/home/ubuntu/.sispopnet/sispopnet.pid
[sispopd]
enabled=true
jsonrpc=127.0.0.1:30000
# network settings
[network]
profiles=/home/ubuntu/.sispopnet/profiles.dat
enabled=true
exit=false
#exit-blacklist=tcp:25
#exit-whitelist=tcp:*
#exit-whitelist=udp:*
ifaddr=10.0.0.144/8
ifname=sispop

# ROUTERS ONLY: publish network interfaces for handling inbound traffic.
# change to eth0 if your cpu support that
[bind]
eth0=1090
[netdb]
dir=/home/<USERNAME>/.sispopnet/netdb

[bootstrap]
# add a bootstrap node's signed identity to the list of nodes we want to bootstrap from
# if we don't have any peers we connect to this router
add-node=/home/ubuntu/.sispopnet/bootstrap.signed
```
* Save the sispopnet.ini file with control X and press y for yes and click on `enter`

```java
Ctrl x
```


* Navigate to sispopnet-r folder and run the sispopnet with

```java
./sispopnet
```

* Exit the screen (detatch)


## Setup the Storage server

* Open another screen for the storage services

```java
screen -dmS storage
```
```java
screen -r storage
```
* Download the storage application

```java
wget https://github.com/sispop-dev/storage/releases/download/v2.0.9/storage.tar.gz
```
* Tar the file with

```java

tar -xzf storage.tar.gz &&

cd storage
```

* Run the storage application as below

```java
./sispop-storage 0.0.0.0 11011 --sispopd-rpc-port=30000
```
* Exit the screen


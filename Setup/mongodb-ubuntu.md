# Mongo DB Setup Instructions (Ubuntu 14)

MongoDB setup instructions for ubuntu can be found [here](https://docs.mongodb.com/v3.2/tutorial/install-mongodb-on-ubuntu/)

## Installation

*  Import public key  
`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927`
*  Create a list file(Ubuntu 14)  
```
echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
```
*  Update local package database  
`sudo apt-get update`
*  Install latest version of mongoDB  
`sudo apt-get install -y mongodb-org`

## Setting up

*  Start the mongod service  
`sudo service mongod start`
*  Issue `tail /var/log/mongodb/mongod.log | grep initandlisten` and verify that there is a line like `[initandlisten] waiting for connections on port 27017`
*  To stop `sudo service mongod stop`
*  To restart `sudo service mongod restart`

## Default locations

*  Data directory `/var/lib/mongodb`
*  Log directory `/var/log/mongodb`
*  Config `/etc/mongod.conf`



 Containerize Mongodb application

Mongodb  is a free and open-source cross-platform document-oriented database program. It is classified as a NoSQL database program.  MongoDB uses JSON-like documents with schemas.

Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package.

In this post, we will install and create a MongoDB database in a docker container using the below commands.

For this lab, I am working on AWS Ubuntu instance.
11)      Launch an AWS Instance  and  login to the system
22)      Install Docker using the below commands.
sudo apt-get -y update
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get -y update
apt-cache policy docker-ce
sudo apt-get install -y docker-ce
sudo systemctl start docker               #use stop to stop the docker
sudo usermod -aG docker username #add the user to the docker group if you wish not to use sudo every time you run docker commands with that user

Logout and login again into the docker when you complete the last step. This step is needed to run docker without sudo.

All of the above commands install docker. Sample docker commands are given below





Let’s write a Dockerfile and install all the necessary parameters to containerize the Mongodb application. Dockerfile steps are given below




FROM ubuntu:latest
MAINTAINER ashoku@qpair.io

RUN \
   apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6 && \
   echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.4.list && \
   apt-get update && \
   apt-get install -y mongodb-org

VOLUME ["/data/db"]
WORKDIR /data

EXPOSE 27017

CMD ["mongod"]

FROM   - specifies the base image you are pulling
MAINTAINER –  Maintainer the app
RUN – command to install mongodb or any other applications.
VOLUME – Mount volume on the container.
WORKDIR – Working Directory
EXPOSE – Expose ports of the container to the localhost
CMD – It is used to open specific shell based on command like python, mongodb

Mention all the above specifications in a Dockerfile and build it using the command
Docker build –t “title” /path/to/dockerfile/

Once the image is built, run it and expose the container port to localhost.

docker run -d -p 27017:27017 –v /data/db:/data/db --name mongodbc  bashokku/mongodb
docker run -d -p local_portnum:27017 –v volume_in_local:volume_in_container --name container_name  image_name

Replace the port number, mounting directory, image according to your needs.
Now the Mongodb container is running on port 27107

Connecting to your MongoDB container

To install mongo_clients use the below command
sudo apt-get install mongodb-clients

Connect to Mongo using
mongo localhost:27017/mydb
mongo localhost:port/db_name

create a movies collection and add rows to it

db.createCollection('movies')
db.movies.insert({ name: 'New York', year: '2015' })
db.movies.insert({ name: 'Paris', year: '2016' })
db.movies.find() Find details in the database


This is how you dockerize Mongodb and access it using Mongo-clients in ubuntu

#Building
docker build -t manticore-sdl-core:master --build-arg CORE_VERSION=master .




docker run -d --name manticore-sdl-core-develop manticore-sdl-core:develop



#Squash
http://jasonwilder.com/blog/2014/08/19/squashing-docker-images/

docker run -d -P -p 9010:8087 -p 9011:12345 --name manticore-core-develop -v ~/dockercorestorage:/usr/build/bin/storage manticore-core-develop


##Large History
docker history russjohnson09/manticore-core-develop


##Run Container
docker run -d -P -p 9010:8087 -p 9011:12345 --name manticore-core-develop -v ~/dockercorestorage:/usr/build/bin/storage russjohnson09/manticore-core-develop:latest

Michaels-MBP:manticore-cores-develop russelljohnson$ docker ps
CONTAINER ID        IMAGE                                         COMMAND                CREATED             STATUS              PORTS                                                                                                                                                                                                                            NAMES
cd28d3704355        russjohnson09/manticore-core-develop:latest   "/bin/bash setup.sh"   23 seconds ago      Up 21 seconds       0.0.0.0:32774->3001/tcp, 0.0.0.0:32773->5050/tcp, 0.0.0.0:32772->5080/tcp, 0.0.0.0:9010->8087/tcp, 0.0.0.0:32771->8090/tcp, 0.0.0.0:32770->8888/tcp, 0.0.0.0:32769->9000/tcp, 0.0.0.0:32768->9898/tcp, 0.0.0.0:9011->12345/tcp   manticore-core-develop


docker commit cd28d3704355

##Find Image
Michaels-MBP:manticore-cores-develop russelljohnson$ docker images
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
<none>                                 <none>              e1c3303420ad        49 seconds ago      4.76GB

##Save Image
docker save e1c3303420ad | sudo docker-squash -t russjohnson09/manticore-core-develop:squash -verbose | docker load




#Flatten Image5037d90b3df3
docker run -d -P -p 9010:8087 -p 9011:12345 --name manticore-core-develop-squash -v ~/dockercorestorage:/usr/build/bin/storage russjohnson09/manticore-core-develop:latest

docker ps
docker export --output=manticore-core-develop-squash.tar manticore-core-develop-squash

cat =manticore-core-develop-squash.tar | docker import - manticore-core-develop-squash:imported

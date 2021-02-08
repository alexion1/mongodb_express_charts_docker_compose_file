# MongoDB with express and Charts Docker Compose file
This is a Docker Compose file to run MongoDB (CE) + MongoExpress(WebUI) + MongoCharts in a single-machine setup without Swarm or Kubernetes.
This is intened to run on Linux, and is not tested for Windows and MacOS. 

[MongoDB](https://www.mongodb.com/) is a modern versatile user-friendly database. Here I use the [official image](https://hub.docker.com/_/mongo) of [MongoDB Community Edition](https://docs.mongodb.com/manual/installation/#mongodb-community-edition-installation-tutorials).
[mongo-express](https://hub.docker.com/_/mongo-express) is a Web-based MongoDB admin interface. 
[Mongo Charts](https://docs.mongodb.com/charts/current/) is a tool for MongoDB analytic.

This script simplifies installation of these tools. If not found, Docker images will be pulled by Docker Compose. These two files (*docker-compose.yml* and *charts-mongodb-uri*) is all you need to setup and run Mongo DB with Charts and web UI.

[Mongo Charts official installation](https://docs.mongodb.com/charts/current/installation) can be tricky. It requires user turning on the Swarm mode, and there are potential difficulty connecting to main Mongo DB. 
* Swarm mode is inteneded for multi-cluster deployment, and it uses more complicated overlay networks. This extra complexity is not needed if you are using a single developement machine. This script connects all three containers with a simple bridge network, making it easy to manage. 
* Another issue of the *official installation* is getting the **Connection String URI**. Because Charts installation is detached from MongoDB installation, it is not clear how to get the MongoDB connection URI. Also this URI may change as you reconfigure MongoDB container during developement. Here because MongoDB and Charts are sitting on the same network, connection between Charts and MongoDB is straight-forward.
* And yet another related *official installation* issue is using the secret. It saves **Connection String URI** as a Docker secret, and secrets are only enabled in the Swarm mode (this is why it is required). Since here connection to MongoDB is defined by Docker-compose parameters, it is saved in the attached *charts-mongodb-uri* file. Just make sure this file is saved together with *docker-compose.yml* in the same place.


## Steps:
1. Make sure that Docker and [Docker Compose](https://github.com/docker/compose) are installed on your host machine. 
2. Create a directory (e.g., mongo-test), copy these two files there (*docker-compose.yml* and *charts-mongodb-uri*), and enter it from your terminal.
3. Type `docker-compose up`
4. That should create 3 containers:
  * mongodb
  * mongo_web_ui
  * mongo_charts
5. Check containers are up and running
  * open http://localhost:8081/ for MongoDB web UI (mongo-express)
  * open http://localhost:80/ for Charts login screen
6. Create Charts user [see step 11 of offcial installation guide](https://docs.mongodb.com/charts/current/installation):
```shell  
docker exec -it \
  $(docker container ls --filter name=_charts -q) \
  charts-cli add-user --first-name "<First>" --last-name "<Last>" \
  --email "<user@example.com>" --password "<Password>" \
  --role "<UserAdmin|User>"
```
7. To connect to database shell, type:
```shell
docker exec -it mongodb mongo
```
Alternatively, you can use web UI to add/delete databases, collections, perform simple CRUD (create, read, update, delete), import/export JSON documents.

## Thanks
* [Using Docker Secrets during Development](https://blog.mikesir87.io/2017/05/using-docker-secrets-during-development/)
* [The Difference Between Docker Compose And Docker Stack](https://vsupalov.com/difference-docker-compose-and-docker-stack/)

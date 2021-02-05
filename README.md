# mongodb_express_charts_docker_compose_file
This is a Docker Compose file to run MongoDB (CE) + MongoExpress(WebUI) + MongoCharts in a single-machine deployment without Swarm or Kubernetes.
This is intened to run on Linux, and is not tested for Windows and MacOS. 

[MongoDB](https://www.mongodb.com/) is a modern versatile user-friendly database. Here I use the [official image](https://hub.docker.com/_/mongo) of Com
[mongo-express](https://hub.docker.com/_/mongo-express) is a Web-based MongoDB admin interface. 
[Mongo Charts](https://docs.mongodb.com/charts/current/) is a tool for MongoDB analytic

Steps:
1. Make sure that Docker and [Docker Compose](https://github.com/docker/compose) are installed on your host machine. 
2. Create a directory (e.g., mongo-test), copy these two files there (docker-compose.yml and charts-mongodb-uri), and enter it from your terminal.
3. Type `<docker-compose up>`
4. That should create 3 containers:
* mongodb
* mongo_web_ui
* mongo_charts


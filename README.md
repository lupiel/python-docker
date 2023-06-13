### Build images

```shell
pip instal Flask
source .venv/bin/activate
(.venv) $ python3 -m flask run
```

```
# build
docker build --tag python-docker .

# list
docker images

# tag
docker tag python-docker:latest python-docker:v1.0.0

# untag/remove image
docker rmi python-docker:v1.0.0
```

### Run containers
```
docker run --publish 8000:5000 python-docker

$curl localhost:8000
Hello, Docker!
```

```
# detatched mode
docker run --detach --publish 8000:5000 python-docker
```


```
docker ps
docker stop <container-id>
docker ps -all
docker restart <container-id>
docker rm <container-id> <container-id>...

#runnin named container
docker run -d -p 8000:5000 --name rest-server python-docker
```

### Develop app
```
docker volume create mysql
docker volume create mysql_config
docker network create mysqlnet
```

```
docker run --rm -d -v mysql:/var/lib/mysql \
  -v mysql_config:/etc/mysql -p 3306:3306 \
  --network mysqlnet \
  --name mysqldb \
  -e MYSQL_ROOT_PASSWORD=p@ssw0rd1 \
  mysql
```

```
docker exec -ti mysqldb mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
...
```

```
pip3 install mysql-connector-python
pip3 freeze | grep mysql-connector-python >> requirements.txt
docker build --tag python-docker-dev .

docker run \
  --rm -d \
  --network mysqlnet \
  --name rest-server \
  -p 8000:5000 \
  python-docker-dev

curl http://localhost:8000/initdb
curl http://localhost:8000/widgets
```

```
# prepare docker-compose.dev.yml
docker compose -f docker-compose.dev.yml up --build
```


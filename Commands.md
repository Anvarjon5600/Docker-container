# Команды для поднятия сайта локально

## Создаем образы

```shell
docker build -t 7_back ./backend
```

```shell
docker build -t 7_database ./database
```

```shell
docker build -t 7_nginx ./frontend
```

## Создаем сеть и вольюм

```shell
docker volume create todo_db
```

```shell
docker network create todo_net
```

# Поднимаем контейнери 
## Если не используем docker-compose, то нам нужно поднимать один за другим.
## 1-Поднимаем базу данных

```shell
docker run --rm -d \
  --name database \
  --net=todo_net \
  -v todo_db:/var/lib/postgresql/data \
  -e POSTGRES_DB=docker_app_db \
  -e POSTGRES_USER=docker_app \
  -e POSTGRES_PASSWORD=docker_app \
  7_database
```

## 2-Поднимаем бекенд

```shell
docker run --rm -d \
  --name backend \
  --net=todo_net \
  -e HOST=database \
  -e PORT=5432 \
  -e DB=docker_app_db \
  -e DB_USERNAME=docker_app \
  -e DB_PASSWORD=docker_app \
  7_back
```

## 3-Поднимаем фронтедн

```shell
docker run --rm -d \
  --name frontend \
  --net=todo_net \
  -p 80:80 \
  -v $(pwd)/nginx/nginx.conf:/etc/nginx/nginx.conf:ro \
  7_nginx
```

## Eсли мы поднимаем с помощью docker-compose.
```shell
 docker-compose up -d
```
### Заходим на сайт [http://localhost](http://localhost) или http:// ип адрес серврера
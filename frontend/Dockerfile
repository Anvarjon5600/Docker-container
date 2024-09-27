FROM ubuntu:20.04 AS build
# Устанавливаем необходимые зависимости, включая curl для загрузки Node.js
RUN apt-get update && apt-get install -y curl

# Устанавливаем Node.js и npm
RUN curl -fsSL https://deb.nodesource.com/setup_16.x | bash - && \
	apt-get install -y nodejs

WORKDIR /app
COPY  ./todo-list/package.json ./package.json
RUN npm install

COPY ./todo-list ./
RUN npm run build

# second stage
FROM nginx
COPY --from=build /app/dist/index.html /nginx/static/index.html
COPY --from=build /app/dist/static/css /nginx/static/
COPY --from=build /app/dist/static/js /nginx/static/
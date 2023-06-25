# docker_task_pro
Задание по Docker PRO

1. Для приложения создан Dockerfile с содержимым:
FROM python:3.8-slim-buster
WORKDIR /app
COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt
COPY . .
CMD ["python3", "app.py"] 

2. Создан фавайл docker-compose.yml:
version: "2.18.1"
services:
  app:
    build: ./app/
    depends_on:
      db:
        condition: service_healthy
    links:
      - db   
  db:
    image: postgres:latest
    ports:
      - 5434:5432
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: testdb
    volumes:
      - ./db/init_scripts/init.sql:/docker-entrypoint-initdb.d/init.sql 
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "postgres"]
      interval: 5s
      retries: 5
    restart: always

  3. Создание образа и запуск контейнеров:sudo docker-compose up


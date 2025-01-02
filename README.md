# BobApp

[![CI - Tests and quality analysis](https://github.com/wilzwert/BobApp/actions/workflows/ci.yml/badge.svg)](https://github.com/wilzwert/BobApp/actions/workflows/ci.yml)
[![Build and push Docker images](https://github.com/wilzwert/BobApp/actions/workflows/docker.yml/badge.svg)](https://github.com/wilzwert/BobApp/actions/workflows/docker.yml)

## Backend
[![codecov](https://codecov.io/github/wilzwert/BobApp/branch/main/graph/badge.svg?token=2RG4Z3WHJU&flag=backend)](https://codecov.io/github/wilzwert/BobApp)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=wilzwert_BobApp_backend&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=wilzwert_BobApp_backend)

[Backend coverage report](https://wilzwert.github.io/BobApp/coverage-backend/)

## Frontend

[![codecov](https://codecov.io/github/wilzwert/BobApp/branch/main/graph/badge.svg?token=2RG4Z3WHJU&flag=frontend)](https://codecov.io/github/wilzwert/BobApp)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=wilzwert_BobApp_frontend&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=wilzwert_BobApp_frontend)

[Frontend coverage report](https://wilzwert.github.io/BobApp/coverage-frontend/)

Clone project:

> git clone https://github.com/wilzwert/BobApp.git

## Front-end 

Go inside folder the front folder:

> cd front

Install dependencies:

> npm install

Launch Front-end:

> npm run start;

### Docker

Build the container:

> docker build -t bobapp-front .  

Start the container:

> docker run -p 8080:8080 --name bobapp-front -d bobapp-front

## Back-end

Go inside folder the back folder:

> cd back

Install dependencies:

> mvn clean install

Launch Back-end:

>  mvn spring-boot:run

Launch the tests:

> mvn clean install

### Docker

Build the container:

> docker build -t bobapp-back .  

Start the container:

> docker run -p 8080:8080 --name bobapp-back -d bobapp-back 
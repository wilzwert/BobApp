# BobApp

[![CI - Tests and quality analysis](https://img.shields.io/github/actions/workflow/status/wilzwert/BobApp/ci.yml?label=CI&logo=Github)](https://github.com/wilzwert/BobApp/actions/workflows/ci.yml)
[![Build and push Docker images](https://img.shields.io/github/actions/workflow/status/wilzwert/BobApp/docker.yml?label=Docker%20Build&logo=Docker)](https://github.com/wilzwert/BobApp/actions/workflows/docker.yml)
[![Frontend coverage](https://img.shields.io/codecov/c/github/wilzwert/BobApp?flag=backend&label=Backend%20coverage&logo=JUnit5)](https://wilzwert.github.io/BobApp/coverage-backend/)
[![Backend Quality Gate Status](https://img.shields.io/sonar/quality_gate/wilzwert_BobApp_backend?server=https%3A%2F%2Fsonarcloud.io&logo=sonarcloud&label=Backend%20quality%20gate)](https://sonarcloud.io/summary/new_code?id=wilzwert_BobApp_backend)
[![Frontend coverage](https://img.shields.io/codecov/c/github/wilzwert/BobApp?flag=frontend&label=Frontend%20coverage&logo=Jasmine)](https://wilzwert.github.io/BobApp/coverage-frontend/)
[![Frontend Quality Gate Status](https://img.shields.io/sonar/quality_gate/wilzwert_BobApp_frontend?server=https%3A%2F%2Fsonarcloud.io&logo=sonarcloud&label=Frontend%20quality%20gate)](https://sonarcloud.io/summary/new_code?id=wilzwert_BobApp_frontend)

[Backend coverage report](https://wilzwert.github.io/BobApp/coverage-backend/)

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
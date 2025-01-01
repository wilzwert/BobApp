# BobApp

## Coverage
[![codecov](https://codecov.io/github/wilzwert/BobApp/branch/main/graph/badge.svg?token=2RG4Z3WHJU)](https://codecov.io/github/wilzwert/BobApp)

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
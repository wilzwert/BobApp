# BobApp

[![Frontend CI](https://img.shields.io/github/actions/workflow/status/wilzwert/BobApp/ci-frontend.yml?label=Frontend%20CI&logo=Github)](https://github.com/wilzwert/BobApp/actions/workflows/ci-frontend.yml)
[![Frontend CD](https://img.shields.io/github/actions/workflow/status/wilzwert/BobApp/cd-frontend.yml?label=Frontend%20CD&logo=Github)](https://github.com/wilzwert/BobApp/actions/workflows/cd-frontend.yml)
[![Backend CI](https://img.shields.io/github/actions/workflow/status/wilzwert/BobApp/ci-backend.yml?label=Backend%20CI&logo=Github)](https://github.com/wilzwert/BobApp/actions/workflows/ci-backend.yml)
[![Backend CD](https://img.shields.io/github/actions/workflow/status/wilzwert/BobApp/cd-backend.yml?label=Backend%20CD&logo=Github)](https://github.com/wilzwert/BobApp/actions/workflows/cd-backend.yml)

[![Backend coverage](https://img.shields.io/codecov/c/github/wilzwert/BobApp?flag=backend&label=Backend%20coverage&logo=JUnit5)](https://wilzwert.github.io/BobApp/coverage-backend/)
[![Backend Quality Gate Status](https://img.shields.io/sonar/quality_gate/wilzwert_BobApp_backend?server=https%3A%2F%2Fsonarcloud.io&logo=sonarcloud&label=Backend%20quality%20gate)](https://sonarcloud.io/summary/new_code?id=wilzwert_BobApp_backend)
[![Frontend coverage](https://img.shields.io/codecov/c/github/wilzwert/BobApp?flag=frontend&label=Frontend%20coverage&logo=Jasmine)](https://wilzwert.github.io/BobApp/coverage-frontend/)
[![Frontend Quality Gate Status](https://img.shields.io/sonar/quality_gate/wilzwert_BobApp_frontend?server=https%3A%2F%2Fsonarcloud.io&logo=sonarcloud&label=Frontend%20quality%20gate)](https://sonarcloud.io/summary/new_code?id=wilzwert_BobApp_frontend)

[Backend coverage report](https://wilzwert.github.io/BobApp/coverage-backend/)

[Frontend coverage report](https://wilzwert.github.io/BobApp/coverage-frontend/)

## Table of contents

- [Clone project](#clone-project)
- [Frontend setup](#frontend-setup)
  - [Docker](#docker-for-frontend) 
- [Backend setup](#backend-setup)
  - [Docker](#docker-for-backend)
- [Frontend CI/CD](#frontend-cicd)
  - [Frontend CI](#frontend-ci)
  - [Frontend CD](#frontend-cd)
- [Backend CI/CD](#backend-cicd)
    - [Backend CI](#backend-ci)
    - [Backend CD](#backend-cd)
- [KPIS](#kpis)
  - [Frontend code coverage](#frontend-code-coverage)
  - [Frontend code quality analysis with SonarQube](#frontend-code-quality-analysis-with-sonarqube)
  - [Backend code coverage](#backend-code-coverage)
  - [Backend code quality analysis with SonarQube](#backend-code-quality-analysis-with-sonarqube)
- [User reviews and issues](#user-reviews-and-issues)
  
      

## Clone project

> git clone https://github.com/wilzwert/BobApp.git

## Frontend setup

Go inside folder the front folder:

> cd front

Install dependencies:

> npm install

Launch Front-end:

> npm run start;

### Docker for frontend

Build the container:

> docker build -t bobapp-front .  

Start the container:

> docker run -p 8080:8080 --name bobapp-front -d bobapp-front

## Backend setup

Go inside folder the back folder:

> cd back

Install dependencies:

> mvn clean install

Launch Back-end:

>  mvn spring-boot:run

Launch the tests:

> mvn clean install

### Docker for backend

Build the container:

> docker build -t bobapp-back .  

Start the container:

> docker run -p 8080:8080 --name bobapp-back -d bobapp-back

## Frontend CI/CD

Two workflows have been set up for frontend CI and CD. 

### Frontend CI
> .github/workflows/ci-frontend.yml

This workflow is designed to run tests and quality analysis on `push` or `pull` request for the frontend, and make reports available.
It may also be run manually (in that case the GitHub action event will be `workflow_dispatch`), which would run the same steps as in a `push` event.

> [!NOTE]
> Some steps are executed differently according to context, because in some cases we want the step to run only when new code has actually been pushed to the main branch. Running manually is considered the same as on push. 
> These steps are indicated below with `workflow_dispatch`, `push`, or `pull_request`.  

A single job `frontend-tests-analysis` is set up, and here are the job's steps : 
1. Checkout repository : checkout code in the  workflow execution environment
2. Set up Node.js : Node.js is needed for frontend testing
3. Install dependencies : install angular project dependencies with npm
4. Install Chrome browser : setup Chrome browser to run tests
5. Run tests with coverage. *This step does not stop job in case of failure, because it could fail on coverage checks, and in that case some other steps should run anyway.* 
6. Verify coverage report existence and stop if missing : in case previous step didn't produce a coverage report, the job should fail, as it indicates a fatal failure.
7. Upload frontend coverage report artifact : make coverage report available as an artifact
8. Publish frontend coverage report to GitHub Pages.`push` `workflow_dispatch`  *We want Pages to reflect the actual main branch coverage state.*
9. Upload frontend coverage report to Codecov. `push` `workflow_dispatch`  *We want Codecov to reflect the actual main branch coverage state.*
10. SonarQube Scan on push to main. `push` `workflow_dispatch`
11. SonarQube Scan on pull request. `pull_request` *In that case, we send the PR info to SonarQue, which adds a comment to the PR.*
12. SonarQube Quality Gate Check
13. Check requirements : fail job if coverage requirements not met. This allows to explicitly specify what went wrong, as we used continue-on-error previously.

> [!NOTE]
> Sonarcloud scan is configured in `./sonar-project-frontend.properties`

### Frontend CD

> .github/workflows/cd-frontend.yml

This workflow is designed to build the frontend docker image and push it to Docker Hub.
It is run when the Frontend CI workflow completes, and may also be run manually.

A single job `frontend-build-and-push` is set up. It is executed when the CI workflow succeeded on a push event, or manually. Here are the job's steps :

1. Checkout repository : checkout code in the  workflow execution environment
2. Setup Node.js
2. Install dependencies and build Angular project 
4. Log in to Docker Hub
5. Extract frontend docker image metadata from Git reference and GitHub events
6. Set up Docker Buildx : for extended build capabilities
7. Build and push frontend image


## Backend CI/CD

### Backend CI
> .github/workflows/ci-backend.yml

This workflow is designed to run tests and quality analysis on `push` or `pull` request for the frontend, and make reports available.
It may also be run manually (in that case the GitHub action event will be `workflow_dispatch`), which would run the same steps as in a `push` event.

> [!NOTE]
> Some steps are executed differently according to context, because in some cases we want the step to run only when new code has actually been pushed to the main branch. Running manually is considered the same as on push.
> These steps are indicated below with `workflow_dispatch`, `push`, or `pull_request`.

A single job `backend-tests-analysis` is set up, and here are the job's steps :
1. Check out repository code in workflow execution environment (VM)
2. Set up JDK 17 : Java is needed for backend testing
3. Install dependencies : install angular project dependencies with npm
4. Run tests and generate coverage report. *This step does not stop job in case of failure, because it could fail on coverage checks, and in that case some other steps should run anyway.*
5. Verify coverage report existence and stop if missing : in case previous step didn't produce a coverage report, the job should fail, as it indicates a fatal failure
6. Upload backend coverage report artifact : make coverage report available as an artifact
7. Publish backend coverage report to GitHub Pages.`push` `workflow_dispatch`*We want Pages to reflect the actual main branch coverage state.*
8. Upload backend coverage report to Codecov. `push` `workflow_dispatch` *We want Codecov to reflect the actual main branch coverage state.*
9. SonarQube Scan on push to main.`push` `workflow_dispatch` *In that case, the analysis is run on the overall codebase, and is reflected by the badge in this README.*
10. SonarQube Scan on pull request. `pull_request` *In that case, the analysis is run on new code only, and adds a comment to the PR.*
11. SonarQube Quality Gate Check.
12. Check requirements : fail job if coverage requirements not met. This allows to explicitly specify what went wrong, as we used continue-on-error previously.

> [!NOTE]
> Sonarcloud scan is configured in `./sonar-project-backend.properties` 

### Backend CD

> .github/workflows/cd-backend.yml

This workflow is designed to build the backend docker image and push it to Docker Hub.
It is run when the Backend CI workflow completes, and may also be run manually.

A single job `backend-build-and-push` is set up. It is executed when the CI workflow succeeded on a push event, or manually. Here are the job's steps :

1. Checkout repository : checkout code in the  workflow execution environment
2. Set up JDK 17
3. Build with Maven
4. Log in to Docker Hub
5. Extract frontend docker image metadata from Git reference and GitHub events
6. Set up Docker Buildx : for extended build capabilities
7. Build and push frontend image

## KPIS

### Frontend code coverage

Code coverage requirements for tests are set up as follows in `./front/karma.conf.js`: 
``` 
  statements: 75,
  branches: 75,
  functions: 50,
  lines: 75
 ```

> [!NOTE]
> As of now current values allow code updates for the current application, but as the overall code quality progresses, we should increase these values to 80%.

### Frontend code quality analysis with SonarQube

Full report of the quality analysis for the main branch frontend is available [here](https://sonarcloud.io/summary/overall?id=wilzwert_BobApp_frontend&branch=main).

A quality gate has been set up with these requirements on new code :

| Metric                     | Operator | Value |
|----------------------------|----------|--------|
| Coverage                   | is less than | 80.0% |
| Duplicated Lines (%)       | is greater than | 3.0% |
| Maintainability Rating     |is worse than|A|
| Blocker Issues             | is greater than | 0     |
| Bugs                       |is greater than|0|
| Critical Issues            |is greater than|0|
| Reliability Rating         |is worse than|A|
| Security Hotspots Reviewed |is less than|100%|
| Security Rating            |is worse than|A|

The quality gate passing status is reflected in the badge on top of this README.
Should the quality gate check fail on push, CI would fail too.

> [!NOTE]
> These values are restrictive enough and allow quality development from now on, but should be fine-tuned as overall code quality increases.

### Backend code coverage

Code coverage requirements for tests ar set up as follows in `./back/pom.xml`:

```xml 
  <limit>
      <counter>INSTRUCTION</counter>
      <value>COVEREDRATIO</value>
      <minimum>0.3</minimum>
  </limit>
 ```

> [!NOTE]
> As of now current requirements are very low but will allow code updates for the current application. As the overall code quality progresses, we should increase these values to 80%.

### Backend code quality analysis with SonarQube

Full report of the quality analysis for the main branch backend is available [here](https://sonarcloud.io/summary/overall?id=wilzwert_BobApp_backend&branch=main).

A quality gate has been set up with requirements below.

For new code : 

| Metric                     | Operator        | Value |
|----------------------------|-----------------|-------|
| Coverage                   | is less than    | 80.0% |
| Duplicated Lines (%)       | is greater than | 3.0%  |
| Maintainability Rating     | is worse than   | A     |
| Blocker Issues             | is greater than | 0     |
| Bugs                       | is greater than | 0     |
| Critical Issues            | is greater than | 1     |
| Reliability Rating         | is worse than   | A     |
| Security Hotspots Reviewed | is less than    | 100%  |
| Security Rating            | is worse than   | A     |

For overall code : 
| Metric                     | Operator        | Value |
|----------------------------|-----------------|-------|
| Coverage                   | is less than    | 30.0% |



The quality gate passing status is reflected in the badge on top of this README.
Should the quality gate check fail on push, CI would fail too.

> [!NOTE]
> The expected code coverage ratio on overall code is very low and should be seen as an adjustable goal to increase overall code quality, as quickly as possible.

## User reviews and issues

User reviews are very negative : 

1. Je mets une une étoile car je ne peux pas en mettre zéro ! Impossible de poster une suggestion de blague, le bouton tourne  et fait planter mon navigateur ! - 1/5
2. #BobApp j'ai remonté un bug sur le post de vidéo il y a deux semaines et il est encore présent ! Les devs vous faites quoi ???? - 2/5
3. Ca fait une semaine que je ne reçois plus rien, j'ai envoyé un email il y a 5 jours mais toujours pas de nouvelles... - 1/5
4. J'ai supprimé ce site de mes favoris ce main, dommage, vraiment dommage.

Issues reported by the app's users should be converted into GitHub issues and addressed as quickly as possible. Users should also get quick feedback on the bugs statuses.

CI/CD pipelines will ensure bug fixes don't actually cause problems and regressions, thus ensuring that the overall user satisfaction increases.   




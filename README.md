# Essential MicroProfile Scaffold

This project uses [MicroProfile](https://microprofile.io/), an open forum to optimize Enterprise Java for a microservices architecture.
It provides a complete **RESTful API** configured, including build, test, and deploy scripts as examples.
It is recommended to have, at least, **Java 11**, [Payara 5](https://www.payara.fish/), and [Docker](https://www.docker.com/) installed.

## Table of Contents

- [Project structure](#project-structure)
- [Running in development mode](#running-in-development-mode)
- [Linting and formatting code](#linting-and-formatting-code)
- [Running unit tests](#running-unit-tests)
- [Running integration tests](#running-integration-tests)
- [Debugging](#debugging)
- [Commit messages convention](#commit-messages-convention)
- [Building and deploying](#building-and-deploying)
- [Reference documentation](#reference-documentation)

## Project structure

When working in a large team with many developers that are responsible for the same codebase, having a common understanding of how the application should be structured is vital.
Based on best practices from the community, other github projects and developer experience, your project should look like this:

```html
├── ci
|  ├── build
|  └── deploy
├── src
|  ├── integration-test
|  ├── main
|  |  ├── java
|  |  |  └── app
|  |  |     ├── AppResource.java
|  |  |     ├── AppService.java
|  |  |     └── JAXRSApplication.java
|  |  ├── resources
|  |  |  └── microprofile-config.properties
|  |  └── webapp
|  └── test
├── .dockerignore
├── .editorconfig
├── .gitignore
├── checkstyle.xml
├── Dockerfile
├── LICENSE
├── mvnw
├── mvnw.cmd
├── pom.xml
└── README.md
```

## Running in development mode

You can run the application on your local [Payara Server](https://www.payara.fish/) instance.
First, you need to build the project. Use:

```bash
./mvnw clean install
```

After, you can deploy the [microprofile-api.war](target/microprofile-api.war) on Payara Server.

This app includes [Swagger](https://swagger.io/). It is available at <http://localhost:8080/microprofile-api/openapi-ui/>.

The OpenAPI Specification is automatically generated on build. See the file [openapi.yaml](target/classes/META-INF/openapi.yaml).

## Linting and formatting code

A linter is a static code analysis tool used to flag programming errors, bugs, stylistic errors and suspicious constructs.

This project includes [Checkstyle](https://checkstyle.sourceforge.io/).
Checkstyle finds class design problems, method design problems, and others. It also has the ability to check code layout and formatting issues.
Use the next command to check your code style.

```bash
./mvnw checkstyle:check
```

## Running unit tests

Unit tests are responsible for testing of individual methods or classes by supplying input and making sure the output is as expected.

Use `./mvnw test` to execute the unit tests via [JUnit 5](https://junit.org/junit5/) and [Mockito](https://site.mockito.org/).

This project uses [JaCoCo](https://www.eclemma.org/jacoco/) which provides code coverage metrics for Java.
The minimum code coverage is set to 80%.
You can see the HTML coverage report opening the [index.html](target/site/jacoco/index.html) file in your web browser.

## Running integration tests

Integration tests determine if independently developed units of software work correctly when they are connected to each other.

Use `./mvnw verify -DskipUTs` to execute the integration tests via [JUnit 5](https://junit.org/junit5/), [Testcontainers](https://www.testcontainers.org/) and [REST Assured](https://rest-assured.io/).
The first time, you need to build the Docker image used in the integration tests. See [Building and deploying](#building-and-deploying) section.

Like unit tests, you can also run subsets of a test suite.

## Debugging

You can debug the source code, add breakpoints, inspect variables and view the application's call stack.
Also, you can use the IDE for debugging the source code and unit tests.

## Commit messages convention

In order to have a consistent git history every commit must follow a specific template. Here's the template:

```bash
<type>(<ITEM ID>?): <subject>
```

### Type

Must be one of the following:

- **build**: Changes that affect the build system or external dependencies (example scopes: Gradle, Maven)
- **ci**: Changes to our CI configuration files and scripts (example scopes: Jenkins, Travis, Circle, SauceLabs)
- **chore**: Changes to the build process or auxiliary tools and libraries such as documentation generation
- **docs**: Documentation only changes
- **feat**: A new feature
- **fix**: A bug fix
- **perf**: A code change that improves performance
- **refactor**: A code change that neither fixes a bug nor adds a feature
- **revert**: A commit that reverts a previous one
- **style**: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc.)
- **test**: Adding missing tests or correcting existing tests

### ITEM ID

The related **issue** or **user story** or even **defect**.

- For **user stories**, you shoud use `US-` as prefix. Example: `feat(US-4321): ...`
- For **no related issues** or **defects** you should leave it blank. Example: `feat: ...`

### Subject

The subject contains a succinct description of the change.

## Building and deploying

This project uses [Maven Release Plugin](https://maven.apache.org/maven-release/maven-release-plugin/) to release a project with Maven.
To create the first SNAPSHOT version, you must execute the commands `./mvnw release:prepare` and `./mvnw release:perform`.

You can generate the changelog from GIT repository via `./mvnw generate-sources` command.

Use the next command to build the Docker image of this application:

```bash
docker build -t microprofile-api .
```

In `ci` folder you can find scripts for your [Jenkins](https://www.jenkins.io/) CI pipeline and an example for deploying your application with [Ansible](https://www.ansible.com/) to [Docker Swarm](https://docs.docker.com/engine/swarm/).
Use the next command to deploy the application to Docker Swarm via Ansible:

```bash
ansible-playbook ci/deploy/deploy-to-swarm.yaml
```

## Reference documentation

For further reference, please consider the following articles:

- [Official Apache Maven documentation](https://maven.apache.org/guides/index.html)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Git Hooks Tutorial](https://www.atlassian.com/git/tutorials/git-hooks)
- [Maven Release Plugin](https://thihenos.medium.com/maven-release-plugin-a-simple-example-of-package-management-9926506acfb9)
- [Boundary Control Entity Architecture](https://vaclavkosar.com/software/Boundary-Control-Entity-Architecture-The-Pattern-to-Structure-Your-Classes)
- [Create a RESTful Web Service with Payara Server](https://blog.payara.fish/create-a-restful-web-service-with-payara-server-netbeans)
- [Using Payara Server with Docker](https://blog.payara.fish/using-payara-server-with-docker)
- [Mockito Tutorial](https://www.baeldung.com/mockito-series)
- [Enforcing code coverage rule with JaCoCo](https://medium.com/@AyushVardhan/enforcing-code-coverage-rule-with-jacoco-in-maven-lifecycle-8ebc1fe3b6ce)
- [Integration Testing with the Payara and Testcontainers](https://blog.payara.fish/jakarta-ee-integration-testing-with-testcontainers)

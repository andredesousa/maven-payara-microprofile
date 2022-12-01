# Essential MicroProfile Scaffold

This project uses [MicroProfile](https://microprofile.io/), an open forum to optimize Enterprise Java for a microservices architecture, and [Payara Micro 5](https://www.payara.fish/).
It was generated with [MicroProfile Starter](https://start.microprofile.io/).
It provides a complete **RESTful API** configured, including build, test, and deploy scripts as examples.
It is recommended to have, at least, **Java 11**, [Node.js](https://nodejs.org/en/) and [Docker](https://www.docker.com/) installed.

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
|  |  ├── docker
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
├── .prettierrc
├── changelog.mustache
├── checkstyle.xml
├── Dockerfile
├── LICENSE
├── mvnw
├── mvnw.cmd
├── pom.xml
└── README.md
```

All of the app's code goes in a folder named `src/main`.
The unit tests and integration tests are in the `src/test` and `src/integration-test` folders.
Static files are placed in `src/main/resources` folder.

## Running in development mode

You can run your application in dev mode that enables live coding.
Use the next command to build and serve your application.

To build and serve the application, you can use the next command:

```bash
./mvnw package payara-micro:start
```

Alternatively, you can run the application on your local [Payara Server](https://www.payara.fish/) instance.
First, you need to build the project with `./mvnw clean package` command.
After build de application, you can deploy the [microprofile-api.war](target/microprofile-api.war) on Payara Server.

This application includes [Swagger](https://swagger.io/).
It is available at <http://localhost:8080/microprofile-api/openapi-ui/>.
The OpenAPI Specification is automatically generated.
See the file [openapi.yaml](target/classes/META-INF/openapi.yaml).

## Linting and formatting code

A linter is a static code analysis tool used to flag programming errors, bugs, stylistic errors and suspicious constructs.

It includes [Prettier](https://prettier.io/), [Checkstyle](https://checkstyle.sourceforge.io/), [PMD](https://pmd.github.io/) and [SpotBugs](https://spotbugs.github.io/):

- **Prettier** enforces a consistent style by parsing your code and re-printing it with its own rules, wrapping code when necessary.
- **Checkstyle** finds class design problems, method design problems, and others. It also has the ability to check code layout and formatting issues.
- **PMD** finds common programming flaws like unused variables, empty catch blocks, unnecessary object creation, and so forth.
- **SpotBugs** is used to perform static analysis on Java code. It looks for instances of "bug patterns".

To enforce all best practices, you can use the next command:

```bash
./mvnw exec:exec -Plint
```

Many problems can be automatically fixed with `./mvnw exec:exec -Pformat` command.
Depending on our editor, you may want to add an editor extension to lint and format your code while you type or on-save.

## Running unit tests

Unit tests are responsible for testing of individual methods or classes by supplying input and making sure the output is as expected.

To execute the unit tests, you can use the next command:

```bash
./mvnw test
```

[JUnit 5](https://junit.org/junit5/) and [Mockito](https://site.mockito.org/) are used in unit tests.
It's a common requirement to run subsets of a test suite, such as when you're fixing a bug or developing a new test case.
To run this through Maven, set the test property to a specific test case via the `./mvnw test -Dtest=SomeTestClass` command.
For more details, you can see the [Running a Single Test](https://maven.apache.org/surefire/maven-surefire-plugin/examples/single-test.html) page of the Maven documentation.

This project uses [JaCoCo](https://www.eclemma.org/jacoco/) which provides code coverage metrics for Java.
The minimum code coverage is set to 80%.
You can see the HTML coverage report opening the [index.html](target/site/jacoco/index.html) file in your web browser.

## Running integration tests

Integration tests determine if independently developed units of software work correctly when they are connected to each other.

To execute the integration tests, you can use the next command:

```bash
./mvnw verify -DskipUTs
```

[JUnit 5](https://junit.org/junit5/), [Testcontainers](https://www.testcontainers.org/) and [REST Assured](https://rest-assured.io/) are used in integration tests.
The first time, you need to build the Docker image used in the integration tests. See [Building and deploying](#building-and-deploying) section.

Like unit tests, you can also run subsets of a test suite.
See the [Running a Single Test](https://maven.apache.org/surefire/maven-failsafe-plugin/examples/single-test.html) page of the Maven documentation.

## Debugging

You can debug the source code, add breakpoints, inspect variables and view the application's call stack.
Also, you can use the IDE for debugging the source code, unit and integration tests.
You can customize the log verbosity of Maven using the `-e` or `-errors` option to see the full stacktrace and the `-X` or `-debug` option to see logs at debug level.

This project includes [Swagger](https://swagger.io/).
To get a visual representation of the interface and send requests for testing purposes go to <http://localhost:8080/microprofile-api/openapi-ui/>.

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

In `ci` folder you can find scripts for your [Jenkins](https://www.jenkins.io/) CI pipeline and an example for deploying your application with [Ansible](https://www.ansible.com/) to [Docker Swarm](https://docs.docker.com/engine/swarm/).

This project uses [Maven Release Plugin](https://maven.apache.org/maven-release/maven-release-plugin/) to release a project with Maven.
To create the first SNAPSHOT version, you must execute the commands `./mvnw release:prepare` and `./mvnw release:perform`.
You can generate the changelog from GIT repository via `./mvnw generate-sources -Pchangelog` command.

This project contains a Dockerfile that you can use to build your Docker image.
To build the Docker image, you can use the next command:

```bash
docker build -f src/main/docker/Dockerfile -t microprofile-api .
```

Then you can verify the built image via `docker images` command.
Also, you can deploy this project to Docker Swarm using `ansible-playbook ci/deploy/deploy-to-swarm.yaml` command.

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

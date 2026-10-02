# TP DevOps Correction Docker

Correction de la partie Docker du module DevOps. Amusez-vous bien avec GitHub Actions !

This project is now mine.

## First CI with backend tests

The [Backend CI workflow](.github/workflows/main.yml) runs on pushes to `main`
and on every pull request. It checks out the repository, sets up Temurin JDK 21
on Ubuntu 24.04, and caches Maven dependencies using `simple-api/pom.xml` as
the cache dependency file. The workflow only requests read access to repository
contents.

The build runs `mvn -B clean verify` inside `simple-api`. Surefire runs unit
tests and Failsafe runs integration tests; a failure in either suite fails CI.
Testcontainers starts a temporary PostgreSQL database using the runner's Docker
daemon. Version 1.21.4 supports recent Docker APIs, fixing the startup failure
caused by the old client's API version 1.32.

To run the same checks locally, install JDK 21 and Maven, start Docker, then run:

```sh
cd simple-api
mvn -B clean verify
```

Results are available in the repository's Actions tab. Local test reports are
written to `simple-api/target/surefire-reports` and
`simple-api/target/failsafe-reports`.

References: [GitHub's Maven CI guide](https://docs.github.com/en/actions/tutorials/build-and-test-code/java-with-maven)
and [Testcontainers 1.21.4 release notes](https://github.com/testcontainers/testcontainers-java/releases/tag/1.21.4).

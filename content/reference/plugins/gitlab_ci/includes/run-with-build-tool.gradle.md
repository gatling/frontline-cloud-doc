```yaml
# Gradle 8 and JDK 17; see https://hub.docker.com/_/gradle for other tags available
image: gradle:8-jdk17

stages:
  - run-gatling-enterprise

run-gatling-enterprise:
  stage: run-gatling-enterprise
  script:
    # See all options on https://gatling.io/docs/gatling/reference/current/extensions/gradle_plugin/#working-with-gatling-enterprise-cloud
    - gradle gatlingEnterpriseStart
      -Dgatling.enterprise.simulationId=00000000-0000-0000-0000-000000000000
      -Dgatling.enterprise.waitForRunEnd=true

# See also https://gitlab.com/gitlab-org/gitlab/-/blob/master/lib/gitlab/ci/templates/Gradle.gitlab-ci.yml
# for other useful options for Gradle builds.
```

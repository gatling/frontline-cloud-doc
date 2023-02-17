```yaml
# Maven 3 and JDK 17; see https://hub.docker.com/_/maven for other tags available
image: maven:3-openjdk-17-slim

stages:
  - run-gatling-enterprise

run-gatling-enterprise:
  stage: run-gatling-enterprise
  script:
    # See all options on https://gatling.io/docs/gatling/reference/current/extensions/maven_plugin/#working-with-gatling-enterprise-cloud
    - mvn --batch-mode gatling:enterpriseStart
      -Dgatling.enterprise.simulationId=00000000-0000-0000-0000-000000000000
      -Dgatling.enterprise.waitForRunEnd=true

# See also https://gitlab.com/gitlab-org/gitlab/-/blob/master/lib/gitlab/ci/templates/Maven.gitlab-ci.yml
# for other useful options for Maven builds.
```

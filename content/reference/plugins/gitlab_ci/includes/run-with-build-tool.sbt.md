```yaml
# SBT 1.8.2 and JDK 17.0.5; sbtscala/scala-sbt does not provide 'latest' tags
# See https://hub.docker.com/r/sbtscala/scala-sbt for other tags available and for the latest versions
image: sbtscala/scala-sbt:eclipse-temurin-17.0.5_8_1.8.2_2.13.10

stages:
  - run-gatling-enterprise

run-gatling-enterprise:
  stage: run-gatling-enterprise
  script:
    # See all options on https://gatling.io/docs/gatling/reference/current/extensions/sbt_plugin/#working-with-gatling-enterprise-cloud
    - sbt Gatling/enterpriseStart
      -Dgatling.enterprise.simulationId=00000000-0000-0000-0000-000000000000
      -Dgatling.enterprise.waitForRunEnd=true
```

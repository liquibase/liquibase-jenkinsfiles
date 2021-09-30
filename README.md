# Liquibase-Jenkins

This repo provides the base Jenkinsfile to be used by any project teams.

Parameters needed to run this groovy script:
- REPO_URL (e.g., git@github.com:acmecorp/moonshot.git)
- BRANCH (e.g., master)
- ENVIRONMENT (e.g., DEV, TEST or PROD)
- CHANGELOG FILE (e.g., masterchangelog.xml)
- CLASSPATH (e.g., postgresql-42.2.23.jar) 
- BASEDIR (The path in repository where sqlcode resides. This is also where changelog and driver file exists)

It expects that your repo consists of the following structure:

- basedir
  - DEV
    - liquibase.properties
  - TEST
    - liquibase.properties
  - PROD
    - liquibase.properties

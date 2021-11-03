# SonarQube with Postgres on docker-compose
Struggling to get a working environment with SonarQube and PostgreSQL?

Use the following docker-compose file and be up and running in minutes.

It is as ‘bare’ as possible:

Use of official Docker images for both PostgreSQL and SonarQube
no other configuration required
use of volumes so you can backup your data
```
version: "3"

services:
  sonarqube:
    image: sonarqube
    restart: unless-stopped
    environment:
      - SONARQUBE_JDBC_USERNAME=sonar
      - SONARQUBE_JDBC_PASSWORD=sonar
      - SONARQUBE_JDBC_URL=jdbc:postgresql://db:5432/sonarqube
    ports:
      - "9000:9000"
      - "9092:9092"
    volumes:
      - sonarqube_conf:/opt/sonarqube/conf
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_bundled-plugins:/opt/sonarqube/lib/bundled-plugins

  db:
    image: postgres
    restart: unless-stopped
    environment:
      - POSTGRES_USER=sonar
      - POSTGRES_PASSWORD=sonar
      - POSTGRES_DB=sonarqube
    volumes:
      - sonarqube_db:/var/lib/postgresql
      - postgresql_data:/var/lib/postgresql/data

volumes:
  postgresql_data:
  sonarqube_bundled-plugins:
  sonarqube_conf:
  sonarqube_data:
  sonarqube_db:
  sonarqube_extensions:
``` 
Start this stack with `docker-compose up -d` You can reach your SonarQube instance at http://localhost:9000Use the default credentials admin/admin to login.

Useful links:

SonarQube on Docker Hub: https://hub.docker.com/_/sonarqube/
PostgreSQL on Docker Hub: https://hub.docker.com/_/postgres/

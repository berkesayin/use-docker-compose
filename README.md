# YAML File For Docker Compose Configuration

#### docker-compose.yml

```yml
version: "3"
services:
  drupal:
    image: bitnami/drupal:latest
    ports:
      - 80:80
      - 443:443
    volumes:
      - drupal_data:/bitnami/drupal
    depends_on: -db
    environment:
      - MARIADB_HOST=db
      - MARIADB_PORT_NUMBER=3306
      - DRUPAL_DATABASE_USER=bn_drupal
      - DRUPAL_DATABASE_PASSWORD=bitnami
      - DRUPAL_DATABASE_NAME=bitnami_drupal
      - DRUPAL_USERNAME=admin
      - DRUPAL_PASSWORD=bitnami
      - DRUPAL_EMAIL=sayinberke34@gmail.com
  db:
    image: bitnami/mariadb:latest
    environment:
      - MARIADB_ROOT_PASSWORD=root_password
      - MARIADB_DATABASE=final
      - MARIADB_USER=YAM428
      - MARIADB_PASSWORD=63987064
    volumes:
      - db_data:/bitnami/mariadb
  webserver:
    image: nginx
    ports:
      - 8080:80
    volumes:
      - drupal_data:/usr/share/nginx/html
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
  certificate:
    image: certbot
    volumes:
      - ./certbot:/etc/letsencrypt
    command: certonly --webroot --webroot-path=/etc/letsencrypt --email sayinberke34@gmail.com --agree-tos --no-eff-email -d sayinberke.me
volumes:
  drupal_data:
  db_data:
```

- This YAML file is a configuration file for Docker Compose, a tool for defining and running multi-container Docker applications. The file defines several services, each of which runs in its own container. Let's break down the different sections:

#### Drupal Service (drupal):

- image: Specifies the Docker image to be used for the Drupal service, in this case, bitnami/drupal:latest.
- ports: Maps the container's ports 80 and 443 to the corresponding ports on the host machine.
- volumes: Mounts a volume named drupal_data to the /bitnami/drupal directory in the container.
- depends_on: Ensures that the drupal service starts only after the db service is up and running.
- environment: Sets environment variables for the Drupal container, specifying configuration details such as the database host, port, user, password, etc.

#### MariaDB Service (db):

- image: Specifies the Docker image for the MariaDB service, in this case, bitnami/mariadb:latest.
- environment: Sets environment variables for configuring the MariaDB container, including the root password, database name, user, and password.
- volumes: Mounts a volume named db_data to the /bitnami/mariadb directory in the container.

#### Nginx Service (webserver):

- image: Specifies the Docker image for the Nginx service, simply using the nginx image.
- ports: Maps the container's port 80 to port 8080 on the host machine.
- volumes: Mounts volumes, including the drupal_data volume used by the Drupal service and an Nginx configuration file (nginx.conf).

#### Certbot Service (certificate):

- image: Specifies the Docker image for the Certbot service, which is used for obtaining SSL certificates.
- volumes: Mounts a volume to store Certbot configuration.
- command: Specifies the Certbot command to obtain a certificate using the webroot method and providing email, domain, and other necessary parameters.

#### Volumes:

- Defines two named volumes (drupal_data and db_data) that are used by the Drupal and MariaDB services, respectively, for persistent data storage.

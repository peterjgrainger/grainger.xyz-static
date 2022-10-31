---
  
  title: Using a Populated MySQL 5.7 (or earlier) as a Codeship Pro Service Without your Tests Breaking
  date: 2020-05-18
---
  
My use case here is to create a service using codeship Pro to start-up and populate a MySQL database then run some tests against that database using my app.

Locally this was working fine starting up the app using `docker-compose` but I needed it to work in the CI pipeline to get those tests passing.  

My first try at this was to use the stock mysql healthcheck image provider by docker and add this as a service.  This image has the healthheck directive in the Dockerfile that runs a script checking that the database can be accessed and returning `exit 0` to let codeship know this container is up and running.

```bash
# codeship-services.yml
mysql:
  image: healthcheck/mysql
  cached: true
  command: mysqld --general-log=0 --log-error
  environment:
    - MYSQL_USER=root
    - MYSQL_ROOT_PASSWORD=admin

```

This worked fine, I got the message `mysql is healthy` and the app started up.  The issue now was the app worked with mysql `5.7` not the latest stable version of mysql `8.0`.

Also in the readme of the `healthcheck/mysql` image it states that this is a prototype image and to not actually use that image for anything.

The solution is to use the SQL server image which does have the healthcheck configured.

```bash
# codeship-services.yml
mysql:
  image: mysql/mysql-server
  cached: true
  command: mysqld --general-log=0 --log-error
  environment:
    - MYSQL_USER=root
    - MYSQL_ROOT_PASSWORD=admin

```

This image is the latest version so you should have no compatibility issues :)have no compatibility issues :)
# Overview of Docker for Development

[![Click here for Overview of Docker for Development](http://img.youtube.com/vi/Hbsf75Do7ws/0.jpg)](http://www.youtube.com/watch?v=Hbsf75Do7ws "Overview of Docker for Development")


Here are the details that will be covered as part of this session.

* Overview of Docker Desktop
* Creating first container
* Creating MySQL Container
  * Specifying Name
  * Binding Port
  * Mounting Directory
* Difference between host and guest (container)
* Running commands in container
* Overview of Docker Images
* Lifecycle of a Container
* Using Dockerfile

```
FROM mysql

ENV MYSQL_ROOT_PASSWORD=itversity

RUN mkdir /data

EXPOSE 3306
```
Run this command to build the image - `docker build . -tag mysql:itversity`

* Characteristics of Docker Container
  * Typically used to run one service.
  * Ephemeral or Stateless
  
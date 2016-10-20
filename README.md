# Docker for Java Developers, from 0 to Dockerized

Repo to accompany my October 2016 talk at the Salt Lake City Java User's Group.  

Slides: https://slides.com/ryanwalls/docker-for-java-developers-from-0-to-dockerized


## Before talk starts
* Install Docker for Mac (https://docs.docker.com/engine/installation/mac/), Windows (https://docs.docker.com/engine/installation/windows/) or Linux (https://docs.docker.com/engine/installation/linux/).


## Dockerizing your app
### Gradle example
Build:
```
docker build -t "gradletest" -f Dockerfile.gradle  .
```

Run:
```
docker run -it -p 8080:8080 gradletest
```

### Maven example
Build:
```
docker build -t "maventest" -f Dockerfile.maven  .
```

Run:
```
docker run -it -p 8080:8080 maventest
```

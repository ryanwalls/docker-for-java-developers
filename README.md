# Docker for Java Developers, from 0 to Dockerized

Repo to accompany my October 2016 talk at the Salt Lake City Java User's Group.  

Slides: https://slides.com/ryanwalls/docker-for-java-developers-from-0-to-dockerized


## Before talk starts
* Install Docker for Mac (https://docs.docker.com/engine/installation/mac/), Windows (https://docs.docker.com/engine/installation/windows/) or Linux (https://docs.docker.com/engine/installation/linux/).


## Dockerizing your app
Look at `Dockerfile.gradle` and `Dockerfile.maven` for examples.

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

When running your build in CI/CD environment, look at https://hub.docker.com/r/ryanwalls/pom-version-parser
for extracting your version number.  (Always tag your docker images!)


## Setting up Jenkins server
*  Create barebones server with some form of linux
*  Open port 80 (you can can do ssl termination with nginx later), I'm running in AWS so I
had to alter my security group.  
*  Install Docker
```
curl -fsSL https://get.docker.com/ | sh
```
If you want to run without sudo, add your user to the docker group
```
sudo usermod -aG docker ubuntu
```
* Create a directory that you'll use to backup your Jenkins config
e.g.
```
mkdir /home/ubuntu/jenkins
```
And make sure the jenkins user and group owns it (On ubuntu box I spun up, `ubuntu` user was already `1000:1000`)
```
sudo chown -R 1000:1000 /home/ubuntu/jenkins
```

* (This could be improved, for demo only!) Let jenkins user access `docker.sock`
```
sudo chmod 777 /var/run/docker.sock
```

* Run jenkins with docker

```
docker run -d --name jenkins -p 80:8080 --restart unless-stopped \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /home/ubuntu/jenkins:/var/jenkins_home \
ryanwalls/jenkins-with-docker:2.19.1-docker1.12.2
```
Copy password that is printed toward end of startup
* Navigate to http://<your host server>
* Go through setup after authenticating with password copied earlier

### Setting up your first build on Jenkins
* Create a new freestyle project

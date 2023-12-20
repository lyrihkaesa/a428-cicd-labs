# simple-node-js-react-npm-app

This repository is for the
[Build a Node.js and React app with npm](https://jenkins.io/doc/tutorials/build-a-node-js-and-react-app-with-npm/)
tutorial in the [Jenkins User Documentation](https://jenkins.io/doc/).

The repository contains a simple Node.js and React application which generates
a web page with the content "Welcome to React" and is accompanied by a test to
check that the application renders satisfactorily.

The `jenkins` directory contains an example of the `Jenkinsfile` (i.e. Pipeline)
you'll be creating yourself during the tutorial and the `scripts` subdirectory
contains shell scripts with commands that are executed when Jenkins processes
the "Test" and "Deliver" stages of your Pipeline.

```bash
docker run \
  --name jenkins-blueocean \
  --detach \
  --network jenkins \
  --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client \
  --env DOCKER_TLS_VERIFY=1 \
  --publish 49000:8080 \
  --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  --volume "$HOME":/home \
  --restart=on-failure \
  --env JAVA_OPTS="-Dhudson.plugins.git.GitSCM.ALLOW_LOCAL_CHECKOUT=true" \
  myjenkins-blueocean:2.361.1-1 
```

```bash
docker run \
  -d \
  --name nginx-reverse-proxy \
  --network jenkins \
  -p 80:80 \
  -p 9000:9000 \
  -v "$HOME/Development/a428-cicd-labs/nginx.conf":/etc/nginx/conf.d/default \
  nginx
```

```bash
docker run \
  -d \
  --name prometheus \
  --network jenkins \
  -p 9090:9090 \
  prom/prometheus
```

```bash
docker run \
  -d \
  --name grafana \
  --network jenkins \
  -p 3030:3030 \
  -e "GF_SERVER_HTTP_PORT=3030" \
  grafana/grafana
```
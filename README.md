# microservice_nodejs

This is a template for a Node.js microservice on Kubernetes.

<br>

Checkout repository:

```
git clone https://github.com/sachsenhofer/microservice_nodejs.git
```

<br>

# App

```
const express = require('express')
const app = express()

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(8080, () => console.log('App listening on port 8080!'))
```

<br>

# Dockerfile

```
FROM node:8.11.2-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 8080

CMD [ "npm", "start" ]
```

<br>

# Usage

### Build

Builds the Docker image.

Execute in terminal:

```
sudo docker build -t microservice-nodejs:latest .
```

<br>

### Push

Pushes an image or a repository to the Google Container Registry.

A Google Cloud account is needed. Replace '__public-207110__' with your own Project-ID.

Execute in terminal:

```
sudo docker tag microservice-nodejs:latest eu.gcr.io/public-207110/microservice-nodejs:latest
sudo docker push eu.gcr.io/public-207110/microservice-nodejs:latest
```

<br>

### Pull

Pulls the image from the Google Container Registry.

Execute in terminal:

```
sudo docker pull eu.gcr.io/public-207110/microservice-nodejs:latest
```

<br>

### Run

Runs the image on your local machine.

Execute in terminal:

```
sudo docker run -p 8080:8080 eu.gcr.io/public-207110/microservice-nodejs:latest
```

<br>

### Deploy

Deploys the application to the Kubernetes cluster.

Execute in terminal:

```
kubectl create -f deploy/kubernetes
```

<br>

Check all ressources that have been created:

```
kubectl get deployments,pods,services -n microservice
```

```
NAME                      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
microservice-nodejs-dep   2         2         2            2           8m

NAME                                       READY     STATUS    RESTARTS   AGE
microservice-nodejs-dep-85bc599557-2g7fj   1/1       Running   0          8m
microservice-nodejs-dep-85bc599557-gzzdz   1/1       Running   0          8m

NAME                      TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)          AGE
microservice-nodejs-svc   LoadBalancer   10.63.253.93    35.233.81.3     8080:31666/TCP   8m
```

<br>

### Delete

To delete all resources, execute the following command.

Execute in terminal:

```
kubectl delete -f deploy/kubernetes
```

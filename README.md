# Service API

The service downloads the first 10 questions on 10 programming languages on the StackOverFlow site.
The obtained data is sent via POST-request to Hastebin service or self-written StorageService
The output is a link where you can see the data in text format

Needed repository:

[ServiceAPI repository](https://github.com/DelsinRow/ServiceApi)

[StorageService repository](https://github.com/DelsinRow/StorageService)

[Documents repository](https://github.com/DelsinRow/ServiceApiDocuments)

## Step 1.Create Docker images of the necessary services


1. Build ServiceAPI & Storage Service images

```
docker build -t storageservice-image 'repository'
docker build -t service-api 'repository'
```

Or go to the repository with the Dockerfile and enter: 
```
docker build -t 'image_name' .
```

```repository``` - PATH to the repository with Dockerfile

```image-name``` - image's name in Docker

2. Build a MongoDB image:

```
docker pull mongo
```


## Step 2-1. Deploy in Docker

Enter command in Terminal from “storage-srvice” directory:

```docker-compose up```

Use the command to output the link to the corresponding service

Hastebin:

```
docker run -e SERVICE=hastebin -e TOKEN='your-token'
 ```

You can get 'your-token' here: [Instruction](https://www.toptal.com/developers/hastebin/documentation)

StorageService:

```
docker run -e SERVICE=storage -e STORAGESERVICE_API_ENDPOINT=http://localhost:8080
```

## Step 2-2. Deploy Services in Kubernetes

First of all deploy ingress nginx. [Instruction](https://kubernetes.github.io/ingress-nginx/deploy/#docker-desktop)

Enter the commands in Terminal from the directory with the yaml files ("Documents" repository):

```
kubectl apply -f 'file-name'
```

Unload the manifests in this order:

1. ```mongodb.yml```

2. ```storageservice.yml```

Enter command:

```
kubectl create configmap myapp-config --from-file=application.yml
kubectl create ingress storageservice-ingress-rule --class=nginx --rule="localhost/*=storageservice:8080”
 ```

Unload one of the manifests(optionally):

```storageservice-job.yml``` or ```hastebin-job.yml```

To work with the Hastebin service, edit the ```hastebin-job``` file. Enter your hastebin-token in the ```value``` field of the TOKEN environment variable. You can get token here: [Instruction](https://www.toptal.com/developers/hastebin/documentation)

To get the desired link, enter the command

```

```
kubectl get pods
```

Find the Pod of Job, you ran earlier. Use its ```pod-name```:

```
kubectl logs 'pod-name'
```

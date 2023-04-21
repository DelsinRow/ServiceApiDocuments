# Service API

The service downloads the first 10 questions on 10 programming languages on the StackOverFlow site.
The obtained data is sent via POST-request to Hastebin service or self-written StorageService
The output is a link where you can see the data in text format

Needed repository:

https://gitlab.mera.com/evsinaev/serviceapi.git

https://gitlab.mera.com/evsinaev/storagesevice-project.git

**Step 1.Create Docker images of the necessary services** 

Enter in the Terminal:

**docker build -t storageservice-image 'repository'**

**docker build -t service-api 'repository'**

where **'repository'** - PATH to the repository with Dockerfile

Or go to the repository with the Dockerfile and enter:

**docker build -t 'image_name' .**

Build a MongoDB image, enter the command:

**docker pull mongo**


**Step 2-1. Deploy in Docker**

Enter command in Terminal from “ServiceAPI” directory:

**docker-compose up**

Use the command to output the link to the corresponding service

Hastebin:

**docker run -e SERVICE=hastebin -e TOKEN='your-token'**

You can get 'your-token' by following the instructions: [Link](https://www.toptal.com/developers/hastebin/documentation)

StorageService:

**docker run -e SERVICE=storage -e API_ENDPOINT_STORAGESERVICE=http://localhost -e FQDN=http://localhost**


**Step 2-2. Deploy in Kubernetes**

First of all deploy ingress nginx. Instruction here: [Link](https://kubernetes.github.io/ingress-nginx/deploy/#docker-desktop)

Enter the commands in Terminal from the directory with the yaml files ("Documents" repository):

**kubectl apply -f 'file-name'**

Unload the manifests in this order:

-mongodb.yml

-storageservice.yml

Enter command:

**kubectl create ingress httpbin-ingress-rule --class=nginx --rule="localhost/*=storageservice:8080”**

Unload one of the manifests(optionally):

-storageservice-job.yml

or

-hastebin-job.yml

To work with the Hastebin service, edit the hastebin-job file. Enter your hastebin-token in the 'value' field of the TOKEN environment variable. You can get it by following the instructions: [Link](https://www.toptal.com/developers/hastebin/documentation)

To get the desired link, enter the command

**kubectl get pods**

Find the Pod of Job, you ran earlier. Copy its 'pod-name'. Enter next command:

**kubectl logs 'pod-name'**

---
layout: post
title: "Kubernetes 101 using ASP.NET Core. Run an ASP.NET core application inside a Kubernetes Pod"
subtitle: "Get started with Kubernetes by running an ASP.NET core application inside a Kubernetes Pod"
date: '2019-11-25 12:08:00 +1000'
background: 'https://source.unsplash.com/random'
---
Kubernetes is a complex topic and it can overwhelm and confuse you the first time. Hence the very explicit title. In this post, I will show you how to run an ASP.NET core application inside a Kubernetes pod. We are not discussing Kubernetes concepts like deployments, namespaces, contexts, load balancer etc. We will start with the simplest of all Pod. A pod in Kubernetes is the smallest execution unit. It runs your container inside a Kubernetes cluster. You can think of it as a wrapper around your container.
## You already have docker and Kubernetes installed.
I assume you already have docker and Kubernetes installed. The latest version of the docker desktop comes with Kubernetes.
## Make sure you have Linux containers selected if using docker for windows
If you have a windows box and you have chosen windows containers, you will find that Kubernetes option is not available in the Docker desktop settings menu. Switch to Linux containers and you can enable Kubernetes from the settings. 
## Create an ASP.NET project with docker
When you select the enable docker support checkbox in Visual Studio, the project will have an extra Dockerfile. Dockerfile is a text file. It contains the Docker commands that you otherwise will run it from the command line to create a docker image. Unfortunately, there are some issues with the Dockerfile Visual Studio generates, which can throw you off the ground, especially if you are a docker newbie. If you look at the Dockerfile below, the copy command tries to copy the csproj file to the directory webapiapp and do a dotnet restore. This will fail, as the Dockerfile and the csproj files are in the same directory.
```
FROM mcr.microsoft.com/dotnet/core/sdk:3.0-buster AS build
WORKDIR /src
COPY ["webapiapp/webapiapp.csproj", "webapiapp/"]
RUN dotnet restore "webapiapp/webapiapp.csproj"
COPY . .
WORKDIR "/src/webapiapp"
RUN dotnet build "webapiapp.csproj" -c Release -o /app/build
```
## Fix the docker file
You can easily move the Dockerfile to the parent directory of your project or change the Dockerfile as shown below. Here we are copying the csproj and all the files in the current directory to the webapiapp directory before executing the dotnet restore command.

```
FROM mcr.microsoft.com/dotnet/core/sdk:3.0-buster AS build
WORKDIR /src
COPY ["webapiapp.csproj", "webapiapp/"]
COPY ./ webapiapp/
RUN dotnet restore "webapiapp/webapiapp.csproj"
```
## Build the docker images
Once you have fixed the Dockerfile execute the command below to build the docker image. This command will build the docker image with name webapiapp and label dev.
```
docker build -t webapiapp:dev .
```
Make sure your image is built. Once the command is successfully executed, verify whether the image is present with the following command.
```
docker images

REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
webapiapp                              dev                 1a1733472871        46 hours ago        207MB
```
## kubectl
```kubectl``` pronounced as "cubecuttle" is the command to interact with Kubernetes. We will use ```kubectl``` to create a pod and run the container image we created above inside the kubernetes cluster.
## Define the pod

The following YAML defines the pod that we want the ```kubectl``` command to create and run inside the Kubernetes cluster. The image property specifies which container image you would like to run inside this pod. As we discussed earlier, consider the pod as a wrapper around your container.

When Kubernetes runs your container inside a pod we are asking Kubernetes to run the web application using the dotnet command through the command property in the YAML file. Substitute the "webapiapp.dll" with the name of your web application dll. 

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: first-pod
  labels:
    app: web
spec:
  containers:
    - name: aspnet-api
      image: webapiapp:dev
      imagePullPolicy: Never
      command: ["dotnet", "webapiapp.dll"]
      ports:
        - containerPort: 80
```
## Create the pod
To create the pod execute the following command. Kubernetes understands that it needs to create a pod from the Kind property in the YAML file. There are several resource types we can create using ```kubectl``` and they are identified by the Kind property.

```
kubectl create -f .\firstpod.yml
```
Make sure the pod is created and is running.
```
kubectl get pod first-pod

NAME        READY   STATUS    RESTARTS   AGE
first-pod   1/1     Running   0          19s
```
## Expose the endpoint
If you see the status of your pod as running,  it means Kubernetes has successfully created your pod and executed the dotnet command. How will you access the web api endpoint from outside? For this, you have to expose and the pod through a port. The following command exposes your pod through a port.
```
kubectl expose pod first-pod --type=NodePort
```
Check the port your service is exposed. To find the port execute the following command. When you asked Kubernetes to expose your pod, it has created a service to expose your pod. The type of service is NodePort as indicated through the --type parameter. A Kubernetes is service is how you expose your pod to the outside world.
```
kubectl get services
```
the above command should produce an output like below
```
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
first-pod    NodePort    10.103.233.217   <none>        80:30581/TCP   16s
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP        2d5h
```
So our web api is exposed through the port 30581.
## Browse the endpoint
```
http://localhost:30581/weatherforecast
```

Voila!! our asp.net core api is running inside a Kubernetes pod
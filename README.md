# Docker OpenCanary
Simple Docker image for OpenCanary in Ubuntu working with python3


# Overview
This image sets up OpenCanary in an Ubuntu container. OpenCanary is a tool that creates honeypot services. Alerts are sent when someone attempts to connect to one of these services.

More details can be found at http://docs.opencanary.org/en/latest/

# Instructions

The `conf` directory contains an `.opencanary.conf` file that specifies services you would like to run. You can also set up alerts through this config file. Leave this file the way it is for a simple example of OpenCanary.

Build an image
```
docker build --rm -t opencanary .
```

Create a container
```
docker run -dit -p 21:21 -p 80:80 --name opencanary-app opencanary</b>
```
View log
```
docker exec -it opencanary-app bash -c 'cat /var/tmp/opencanary.log'
```

Run container with persistent logs
```
docker run -dit -p 80:80 --name opencanary-app -v /path/to/logs/folder/:/var/tmp/ opencanary
```

# Kubernetes

Want to play with Kubernetes?
After building your Dockerfile run this

```
docker tag opencanary:latest dockerregistry:5000/opencanary
docker push dockerregistry:5000/opencanary
```

Open and edit the mount path in kube-opencanary.yaml.
By default, kube-opencanary.yaml is configured to create ftp and http services.
Edit your ports if needed.
Make sure your Docker registry is named properly.  The default is set to pull from server `dockerregistry:5000`.

Run:
```
kubectl create -f kube-opencanary.yaml
kubectl create -f kube-opencanary-service.yaml
```

Your pod should now be running.  Check the status with

```
kubectl get all
kubectl describe pods
```

Get your pod name and use it to enter the shell

```
kubectl get pods
kubectl exec -it opencanary-xxxxxxxxxx-xxxxx bash
```
Start OpenCanary and your are finished
```
opencanaryd --start
exit
```

View logs
```
docker exec -it opencanary-app sh -c 'cat /var/tmp/opencanary.log'
```

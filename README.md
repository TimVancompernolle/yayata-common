# yayata-common

Repository for locally bootstrapping 925r and Yayata.

## Dependencies

- [Git](https://git-scm.com/) - [Installation](https://git-scm.com/download/linux)
- [Taskfile](https://taskfile.dev/) - [Installation](https://taskfile.dev/installation/#install-script)
- [Docker with Compose v2](https://docs.docker.com/compose/) - [Installation](https://docs.docker.com/compose/install/linux/) - make sure docker can run without sudo 
- [Helm](https://helm.sh/) - [Installation](https://helm.sh/docs/intro/install/)
- [k3d](https://k3d.io/v5.7.4/) - [Installation](https://github.com/k3d-io/k3d) (See 'get' section)
- [Kubectl](https://kubernetes.io/docs/reference/kubectl/) - [Installation](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

## Installation

Create a new directory for all the 925r and yayata repositories to live in:

```
mkdir -p ~/inuits/yayata-application
```

Cd to that directory

```
cd ~/inuits/yayata-application
```

Clone this repository:

```
git clone https://github.com/TimVancompernolle/yayata-common.git
```

Step inside the new directory.

```
cd yayata-common
```

Excecute the following task: 

This task will :
- Clone the correct repositories 
- Build the docker image for 925r and yayata. 
- Create a k3d cluster 
- Create a k3d image repo
- Push the 925r and yayata image to that image repo
```
task create-cluster
```

If this task is completed do: 
```
task start
```
__DISCLAIMER__: The first time you do ``` task start ``` the ninetofiver pod will crashloop for a while until the LDAP and MYSQL pods are up. 
These pods spin up a bit slower because their images need to be pulled from an online repo the first time.

To check on your running pods you can do:
```
kubectl get pods -w
```
 When all pods are running: 
 
Open your browser on [http://ninetofiver.localhost](http://ninetofiver.localhost) for the 925r application.

Open your browser on [http://yayata.localhost](http://yayata.localhost) for the Yayata application.

The default credentials for Yayata and 925r are admin admin (logging in for the first time into yayata and 925r also takes a little while not sure why)

If you want to stop and remove the pods: 

```
task stop
```

## Add minio s3 as media bucket
If you want the media files to be stored in an S3 like bucket locally execute the following steps: 

Run the following command to spin up the minio server & to create the bucket. 
```
task start-minio
```

Make sure to uncomment the code lines 354-375 in `yayata-application/925r/ninetofiver/settings.py` 

Once the pods are running browse to http://minio.localhost/
The credentials are: 
```
Username: minioadmin
Password: minioadmin
```

You can test the connection by adding a company in 925r and loading up a picture as logo. 
After that you will see the logo in the media files bucket on minio. 

To stop & remove the minio pod: 
```
task stop-minio
```


## Troubleshoot
Check the ID of a pod with the following command:
```
kubectl get pods -w
```
Check logs for a pod with the following command:
```
kubectl logs [podIdentifier]
```
Check the description of a pod with the following command: 
```
kubectl describe pod [podIdentifier]
```

## Configuration

If you want to change the configuration of 925r or the mysql application

```
vi yayata-common/chart/values.yaml
```
Or you can fiddle with the helm charts in
```
vi yayata-common/chart/templates
```

Make sure to run the following command after changing the charts or values file
```
task upgrade
```

If you change settings in the 925r or yayata directories, the images have to be rebuilt. 
This can be done by: 
```
task remove-cluster
task create-cluster
```

# yayata-common

Repository for locally bootstrapping 925r and Yayata.

## Dependencies

- [Git](https://git-scm.com/) - [Installation](https://git-scm.com/download/linux)
- [Taskfile](https://taskfile.dev/) - [Installation](https://taskfile.dev/installation/#install-script)
- [Docker with Compose v2](https://docs.docker.com/compose/) - [Installation](https://docs.docker.com/compose/install/linux/) - make sure docker service and socket is running
- [Helm](https://helm.sh/) - [Installation](https://helm.sh/docs/intro/install/)
- [k3d](https://k3d.io/v5.7.4/) - [Installation](https://github.com/k3d-io/k3d) (Use install script on github)
- [Kubectl](https://kubernetes.io/docs/reference/kubectl/) - [Installation](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

## Installation

Create a new directory for all the 925r and yayata repositories to live in:

```
mkdir -p ~/inuits/yayata-application
```

Cd to that repo

```
cd ~/inuits/yayata-application
```

Clone this repository [GitHub]():

```
git clone --branch feature/metalarend/local-setup --single-branch https://github.com/inuits/yayata-common.git
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

This will spin up the yayata application with the help of Helm

Open your browser on [http://ninetofiver.localhost](http://ninetofiver.localhost) for the 925r application.

Open your browser on [http://yayata.localhost](http://yayata.localhost) for the Yayata application.

The credentials for Yayata can be found in the 925r .env file.

If you want to stop and remove the pods: 

```
task stop
```
## Monitor

To check on your running pods you can do:
```
kubectl get pods
```

```
kubectl logs [podIdentifier]
```

```
kubectl describe pod [podIdentifier]
```

## Configuration

If you want to change the configuration of 925r or the mysql application

```
vi yayata-common/task/chart/values.yaml
```


### Compose

Start both 925r and Yayata:

```
task start
```
```
task stop
```         
Open your browser on [http://localhost:8000](http://localhost:8000) for the 925r application.

Open your browser on [http://localhost:8080](http://localhost:8080) for the Yayata application.

The credentials for Yayata can be found in the 925r .env file.

For more details, check the 925r and Yayata README.md files.
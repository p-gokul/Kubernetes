# Todo Application Kubernetes Deployment

Kubernetes deployment of full stack Todo app on production. Pacing deployment using Helm and Kustomize.

## Project Overview

The Todo application consists of:

- **Frontend**: Next.Js UI for the Todo application
- **Backend**: API server that handles Todo operations
- **Database**: PostgreSQL database for storing Todo items
- **Prisma**: Database ORM with migration capabilities

## Deployment Options

Three Ways of Kubernetes Deployment are listed below
- Plain Kubernetes : Deploy to kubernetes using the command ```kubectl apply -f file_name.yaml```
- Helm : Faster and Convenient way of kubernetes deployment.
- Kustomize : Kubernetes inbuilt way of faster deployment of apps in kubernetes.

#### Note ::
- I have used the k3s distro for deployment. You can install the k3s cluster from the official site using install script [https://docs.k3s.io/quick-start](https://docs.k3s.io/quick-start)

## 1. Plain Kubernetes

Note ::
1. Update your domain name in the configmap and ingress file to ```your-domain.com```.
2. Make sure the domain are pointing to the public ip of the kubernetes cluster. Use `dig your-domain.com` to find out.

#### a. Clone the repository 

```
git clone https://github.com/p-gokul/Kubernetes.git
```

#### b. Goto k8s directory
```
cd k8s
```

#### c. Run the below commands to execute.
```
k apply -f configmap.yaml
k apply -f secret.yaml
k apply -f pvc.yaml
k apply -f postgres.yaml
k apply -f job-prisma-migrate.yaml
k apply -f backend.yaml
k apply -f frontend.yaml
k apply -f ingress.yaml
```

✨ Now you can access the todo app on ```http://your-domain.com``` 

## 2. Helm

Prerequisites :: 
- K8s cluster should have helm pre-installed. Check this official site to install [https://helm.sh/docs/intro/install/](https://helm.sh/docs/intro/install/) <br>
Use command ```helm version``` to check helm version.
- Update the origin and apiUrl in config, and frontendHost and backendHost to your custom ```your-domain.com``` and ```your-domain-api.com```. Use ```dig``` command to confirm.

#### a. Goto helm directory
```
cd helm
```

#### b. Run the below command to install the todo-app.
```
helm install todo-app .
```
💥 Now you can access the todo app on ```http://your-domain.com```

## 3. Kustomize

Modern ```kubectl``` tool comes with inbuilt kustomize. Thus no need to install anything. 

PreRequisites ::
- Replace values ORIGIN and API_URL at overlays/prod/configmap.yaml to your custom ```your-domain.com``` and ```your-domain-api.com```. Replace host with same values at overlays/prod/ingress.yaml for frontend and backend respectively.

#### a. Goto kustomize directory
```
cd kustomize
```

#### b. Create a namespace todo
```
kubectl create ns todo
``` 

#### c. Run the below command to deploy

```
kubectl apply -f ./overlays/prod/
```

🔥 Now you can access the todo app on ```http://your-domain.com```

# Kubernetes

`$ docker build -t kube-first-app .`

`$ docker tag kube-first-app sarfarazsajjad/kube-first-app`

`$ docker push sarfarazsajjad/kube-first-app`

`$ kubectl create deployment first-app --image=sarfarazsajjad/kube-first-app`

`$ kubectl get deployments`

`$ kubectl get pods`

`$ minikube dashbaord`

`$ kubectl expose deployment first-app --type=LoadBalancer --port=8080`

`$ kubectl get services`

```
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
first-app    LoadBalancer   10.97.193.118   <pending>     8080:32534/TCP   25s
kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP          15d
```

`$ minikube service first-app`

```
|-----------|-----------|-------------|---------------------------|
| NAMESPACE |   NAME    | TARGET PORT |            URL            |
|-----------|-----------|-------------|---------------------------|
| default   | first-app |        8080 | http://192.168.49.2:32534 |
|-----------|-----------|-------------|---------------------------|
🏃  Starting tunnel for service first-app.
|-----------|-----------|-------------|------------------------|
| NAMESPACE |   NAME    | TARGET PORT |          URL           |
|-----------|-----------|-------------|------------------------|
| default   | first-app |             | http://127.0.0.1:58922 |
|-----------|-----------|-------------|------------------------|
🎉  Opening service default/first-app in default browser...
❗  Because you are using a Docker driver on darwin, the terminal needs to be open to run it.
```

`$ kubectl scale deployment/first-app --replicas=3`

```
deployment.apps/first-app scaled
```

`$ kubectl get pods`

```
NAME                         READY   STATUS              RESTARTS   AGE
first-app-69c66bfdf8-hfmlj   0/1     ContainerCreating   0          6s
first-app-69c66bfdf8-kpmj4   1/1     Running             0          6s
first-app-69c66bfdf8-nssht   1/1     Running 
```

## do some code update and update deployment

`docker build -t sarfarazsajjad/kube-first-app:2 .`

`docker push sarfarazsajjad/kube-first-app:2`

`kubectl set image deployment/first-app kube-first-app=sarfarazsajjad/kube-first-app:2`

`kubectl rollout status deployment/first-app`


## deployment rollback and history

Create a wrong deployment update that will not work
`$ kubectl set image deployment/first-app kube-first-app=sarfarazsajjad/kube-first-app:3`

### Undo
`$ kubectl rollout undo deployment/first-app`
### Check status
`$ kubectl rollout status deployment/first-app`
### Check history
`$ kubectl rollout history deployment/first-app`
```
deployment.apps/first-app 
REVISION  CHANGE-CAUSE
1         <none>
3         <none>
4         <none>
```
### Check detail of specific rollout history
`$ kubectl rollout history deployment/first-app --revision=3`

```
deployment.apps/first-app with revision #3
Pod Template:
  Labels:	app=first-app
	pod-template-hash=68f496d9cc
  Containers:
   kube-first-app:
    Image:	sarfarazsajjad/kube-first-app:3
    Port:	<none>
    Host Port:	<none>
    Environment:	<none>
    Mounts:	<none>
  Volumes:	<none>
```
### rollout to specific point in history 

`$ kubectl rollout undo deployment/first-app --to-revision=1`

`$ deployment.apps/first-app rolled back`

### cleanup

`$ kubectl delete service first-app`

`$ kubectl delete deployment first-app`
Ein Deployment verwaltet eine Gruppe von Pods, um eine Anwendungs-Workload auszuführen, in der Regel eine, die keinen Zustand verwaltet.

https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

`k create deployment test --image=httpd --replicas=4`

  * erstelle 4 Replicas mit dem deployment
  * Deployment steuert die Replicasets die im Deployment erstellt werden
	    `k get deployment.apps`
		`k describe replicasets.apps` test-x
  * wir können damit eine gewünschte Anzahl an Replicas eines Images erstellen
  * ==Replicasets werden nur vom deployment und kubernetes erstellt, verändert, gemanaged es gibt kein `k create replica` oder ähnliches==
  * man kann ein deployment haben mit mehreren ReplicaSets mit gleichen Label, wo die alten weitergeführt werden und neue 

`k create deployment -h | less`

`k create deployment test --image=httpd --replicas=10 --dry-run=client -o yaml > deploy.yaml`

`k apply -f deploy.yaml`

`k get deployment.apps`

`k describe deployment.apps`

  Deployments ist ein high-level Konzept das Replicas steuert und erstellt und Deklarationen bzw. Änderungen für die PODS bereithält. Deployment erzeugen selber nicht die PODS, sondern steuert nur die ReplicaSets. Die Doku empiehlt selber

```terminal

[birneos@vmd20921]~/projekt/kubernetes% k apply -f deploy-nginx.yaml
deployment.apps/test-nginx created

[birneos@vmd20921]~/projekt/kubernetes% k get pods

NAME                         READY   STATUS    RESTARTS   AGE
nginx-56c45fd5ff-qx2bk       1/1     Running   0          21h
test-nginx-9496d7c6b-2wz8q   1/1     Running   0          4s
test-nginx-9496d7c6b-8495z   1/1     Running   0          4s
test-nginx-9496d7c6b-9gv74   1/1     Running   0          4s
test-nginx-9496d7c6b-b8pqt   1/1     Running   0          4s
test-nginx-9496d7c6b-k25p5   1/1     Running   0          4s
test-nginx-9496d7c6b-kbpcs   1/1     Running   0          4s
test-nginx-9496d7c6b-l4nkf   1/1     Running   0          4s
test-nginx-9496d7c6b-n6llz   1/1     Running   0          4s
test-nginx-9496d7c6b-pcgt5   1/1     Running   0          4s
test-nginx-9496d7c6b-vcxmq   1/1     Running   0          4s

```
  

 **Was sind Replicas?**
https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/

 Wir der Name schon sagt ein Set von Replicas eines bestimmten Pods. ReplicaSets werden komplett von Deployments gemanaged

Schau dir so ein Deployment mit `"k describe deployment.apps"` selber an. Dort werden die Replicasets deklariert, die für das Deployment genutzt werden

```
[birneos@vmd20921]~/projekt/kubernetes% k describe deployments.apps

Name:                   nginx
Namespace:              default
CreationTimestamp:      Thu, 07 May 2026 12:33:17 +0200
Labels:                 app=nginx
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=nginx
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:         nginx
    Port:          <none>
    Host Port:     <none>
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   nginx-56c45fd5ff (1/1 replicas created)
Events:          <none>


Name:                   test-nginx
Namespace:              default
CreationTimestamp:      Fri, 08 May 2026 10:43:21 +0200
Labels:                 app=test-nginx
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=test-nginx
Replicas:               10 desired | 10 updated | 10 total | 10 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=test-nginx
  Containers:
   nginx:
    Image:         nginx
    Port:          <none>
    Host Port:     <none>
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   test-nginx-9496d7c6b (10/10 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  76s   deployment-controller  Scaled up replica set test-nginx-9496d7c6b from 0 to 10

```


#### spec:Strategy
Spezifiziert im Deployment die Strategie um wie alte Pods durch neue ersetzt werden. Siehe auch [[Strategy]] 

#### Recreate Deployment[](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#recreate-deployment)

All existing Pods are killed before new ones are created when `.spec.strategy.type==Recreate`.



Nachdem wir deployt haben, ändern wir einmal das Image für unser deployment, vielleicht wollen wir mal eine ältere Version httpd (Apache) ausprobieren mit dem Tag ( alpine3.22)


https://hub.docker.com/_/httpd
https://hub.docker.com/_/httpd/tags


![[Pasted image 20260508105255.png]]


![[Pasted image 20260508110438.png]]

Anpassung unseres Deployment der  *deploy-httpd.yaml*

![[Pasted image 20260508111115.png]]

Anwenden des neuen Deployments und in wenigen Sekunden, werden alte Pods beendet, neue Container erstellt und gestartet....wow

`k apply -f deploy-httpd.yaml`


tmux mit 2 horizontalen Screens

```
<STRG>+<b> "  erstellt 2 horizontale Screens
<STRG>+<b> %  erstellt 2 vertikale Screens

im oberen Screen mit Linux Befehl watch arbeiten
watch -n 1 "kubectl get pods" lassen wir jede Sekunde get pods ausgeben

im unteren Bildschirm das Deployment anpassen und applyen 
```


![[Pasted image 20260508112248.png]]



### Wir simulieren einen Fehler beim Deployment

Beim starten eines containers, Pods oder ein Deployment kann auch ein Command ausgeführt werden.

Wir fügen zum Test folgendes command und arguments hinzu. Das führt die Bash-Shell aus, 5s Pause und dann mit Fehler Code 1 aussteigen
```yaml
 spec:
      containers:
      - image: nginx
        name: nginx
        command: ["/bin/bash","-c"] #override the default command
        args: ["sleep 5; exit 1"] # sleep for 30 seconds then exit with error
        resources: {}
```


Unser Container kann also nicht zu Ende ausgeführt werden, was nun ?

```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: test-nginx
  name: test-nginx
spec:
  replicas: 10
  selector:
    matchLabels:
      app: test-nginx
  strategy: 
	type: RollingUpdate
    rollingUpdate:
     maxSurge: 1
     maxUnavailable: 1
  template:
    metadata:
      labels:
        app: test-nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        command: ["/bin/bash","-c"] #override the default command
        args: ["sleep 5; exit 1"] # sleep for 30 seconds then exit with error
        resources: {}
status: {}


```



##### Unser Container kann also nicht erstellt werden, was nun ?

Alle anderen Instanzen die noch nicht ausgetauscht wurden laufen weiter und werden vom neuen Deployment nicht in Mitleidenschaft gezogen....genial. Dadurch das unsere Strategie RollingUpdate ist und maxSurge (also max 1 mehr als definiert und definiert sind ja 10 Pods also sind max 11 möglich , d.h. 2 dürfen  max gleichzeitig getauscht werden)

Wir können das Deployment anpassen und neu ausführen.

![[Pasted image 20260508135439.png]]

`k delete deployments.app test-httpd` entfernt das komplette Deployment

![[Pasted image 20260508142407.png]]

Wir entfernen auch denen einen noch laufenden Pod den wir einzeln erstellt hatten
mit `k create pod nginx --image=nginx`
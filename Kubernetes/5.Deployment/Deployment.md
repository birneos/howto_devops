
https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

  Wir könnnen damit eine gewünschte Anzahl an Replicas eines Images erstellen

`k create deployment -h | less`

`k create deployment test --image=httpd --replicas=4`

  

`k create deployment test --image=httpd --replicas=10 --dry-run=client -o yaml > deploy.yaml`

`k apply -f deploy.yaml`

`k get deployment.apps`

`k describe deployment.apps`

  Deployments ist ein high-level Konzept das Replicas steuert und erstellt und Deklarationen bzw. Änderungen für die PODS bereithält. Deployment erzeugen selber nicht die PODS, sondern steuert nur die ReplicaSets. Die Doku empiehlt selber

  



 **Was sind Replicas?**
https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/

 Wir der Name schon sagt ein Set von Replicas eines bestimmten Pods. ReplicaSets werden komplett von Deployments gemanaged

Schau dir so ein Deployment mit "k describe deployment.apps" selber an. Dort werden die Replicasets deklariert, die für das Deployment genutzt werden

```

    Name: test

    Namespace: default

    CreationTimestamp: Thu, 07 May 2026 14:55:03 +0200

    Labels: app=test

    Annotations: deployment.kubernetes.io/revision: 1

    Selector: app=test

    Replicas: 5 desired | 5 updated | 5 total | 5 available | 0 unavailable

    StrategyType: RollingUpdate

    MinReadySeconds: 0

    RollingUpdateStrategy: 25% max unavailable, 25% max surge

    Pod Template:

    Labels: app=test

    Containers:

    nginx:

    Image: nginx

    Port: <none>

    Host Port: <none>

    Environment: <none>

    Mounts: <none>

    Volumes: <none>

    Node-Selectors: <none>

    Tolerations: <none>

    Conditions:

    Type Status Reason

    ---
    Available True MinimumReplicasAvailable

    Progressing True NewReplicaSetAvailable

    OldReplicaSets: <none>

    NewReplicaSet: test-6bb654b8f8 (5/5 replicas created)

    Events:

    Type Reason Age From Message
    ---
    Normal ScalingReplicaSet 46s deployment-controller Scaled up replica set test-6bb654b8f8 from 0 to 5

```
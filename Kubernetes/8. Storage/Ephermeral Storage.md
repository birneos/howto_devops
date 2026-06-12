*  Volumes können vorher oder dynamisch erstellt werden

Wir erstellen uns einen einfachen POD und legen ein leeres **emptyDir** Volume an. 

Bsw. wir brauchen nur einen Zwischenspeicher um Informationen abzulegen.

Achte darauf:  wenn **volume:** auf container Ebene eingerückt ist wird ein Volume auf Pod Ebene erstellt, wenn volume weiter eigenrückt wird steht es auf Container-Ebene

mountPath:  Location des volume in dem Container

```
## Beispiel:  empty Volume und in der Containerkonfiuration geben wir an welches 
              Volume genutzt werden soll, es können hier ja auch mehrere Volumes                 und auch mehrere Container definiert werden
```
```

```yaml

apiVersion: v1
kind: Pod
metadata:
  labels:
  name: test-nginx
spec:
  containers:
    - image: nginx
      name: nginx
      volumeMounts:
        - mountPath: /scratch
          name: scratch-volume
  volumes:
    - name: scratch-volume
      emptyDir:
           sizeLimit: 500Mi
```


```
1. Pod erstellen
   k apply -f nginx-storage.yaml
   
2. Pods anzeigen
   k get pods
   
	NAME                      READY   STATUS    RESTARTS   AGE
	mealie-584796f585-79phk   1/1     Running   0          8h
	storage-nginx             1/1     Running   0          11s

   
3. In den Container
   k exec -it nginx-storage -- bash

ls -lah
...
drwxrwxrwx   2 root root 4.0K Jun  9 13:30 scratch
...
```





# Schau dir mal einen Pod an was bei Volumes normalerweise steht, wenn wir kein Volume erstellen

k describe pod mealie-584796f585-79phk |less

```yaml

Name:             mealie-584796f585-79phk
Namespace:        mealie
Priority:         0
Service Account:  default
..
..
..
Volumes:
  kube-api-access-psx9m:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:                      <none>

```




```
```

Wir erstellen nun einen Service für unsere Mealie Anwendung. Unabhängig welche Pods herunter- oder hochgefahren werden, unser Service ist immer ansprechbar und leitet die Anfragen an die verfügbaren Pods weiter.

Wir erstellen einen Service und forwarden den Port nach außen über die unseren Anwendung aufrubar sein soll.

```
# Service erstellen
kubectl expose deployment mealie --port 9000

# Service forwarden, um von außen erreichbar zu sein
`k port-forward services/mealie 9000:9000`
```
### Service Forwarding Alternative 

Wenn wir das Fenster mit dem Fowarding schliessen können wir den Dienst nicht mehr erreichen, daher werden wir einen LoadBalancer Service erstellen der diesen Service im Cluster an einem Port zur Verfügung stellt.

![[Pasted image 20260609083346.png]]


### Service yaml erstellen

Wir hatten einen Mealie Service erstellt und geben die yaml dazu aus und erstellen daraus unseren
eigenen Servicem mit Loadbalancer als Type, der dann einen Port expost.

`k get service meaelie -o yaml > mealie-service.yaml`


Original
```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: "2026-06-09T06:30:11Z"
  labels:
    app: mealie
  name: mealie
  namespace: mealie
  resourceVersion: "1001977"
  uid: 91dc0bfc-d09c-4048-aac3-e4389941ec48
spec:
  clusterIP: 10.43.129.15
  clusterIPs:
  - 10.43.129.15
  internalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - port: 9000
    protocol: TCP
    targetPort: 9000
  selector:
    app: mealie
  sessionAffinity: None
  type: ClusterIP
status:
  loadBalancer: {}

```

### Mealie Service

**mealie-service.yaml**
```yaml
## Angepasste Version mit Loadbalancer
## 
apiVersion: v1
kind: Service
metadata:
  labels:
    app: mealie
  name: mealie
  namespace: mealie
spec:
  ports:
  - port: 9000
    protocol: TCP
    targetPort: 9000
  selector:
    app: mealie
  type: LoadBalancer

```


Wir sehen folgend das nun bei EXTERNAL-IP eine IP steht, über die unsere Anwendung von außen aufgerufen werden kann, ohne ein Port-Forwarding auf den Service vorzunehmen.

```terminal
k apply -f mealie-service.yaml
service/mealie created

k get svc
NAME     TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
mealie   LoadBalancer   10.43.200.1   173.212.228.65   9000:31218/TCP   7s

```
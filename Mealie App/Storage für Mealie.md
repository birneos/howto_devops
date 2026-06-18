Wir möchten für unsere Mealie-App persistenten Speicher nutzen, d.h. auch wenn der Mealie Pod (deployment) neue Pods erstellt etc. soll die Daten der Anwendung gespeichert bleiben.


```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mealie-data
  namespace: mealie
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi

```


### Referenz zum Volume 
Im Deployment verwenden wir den Referenznamen **mealie-data** als Volume

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: mealie
  name: mealie
  namespace: mealie
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mealie
  template:
    metadata:
      labels:
        app: mealie
    spec:
      containers:
      - image: nginx
        name: nginx
        
```

```bash
# Wenn das Deployment nun um das Persisent Volume Claim erweitert wurde

k apply -f mealie-deployment.yaml

k get persitentvolumeclaim

# Wir sehen das persisten volume claim hat nun 500 MB gebunden

NAME          STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
mealie-data   Bound    pvc-b535859c-f87b-443f-a75e-d84ca9cfad17   500Mi      RWO            local-path     <unset>                 50m

```

#### mealie original yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: mealie
  name: mealie
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mealie
  strategy: {}
  template:
    metadata:
      labels:
        app: mealie
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}

```

mealie-deployment.yaml
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

Deployment mit Storage
##### siehe Referenz zum Volume in [[Storage für Mealie]]

* Die Volumes befinden sich auf POD-Ebene
	* volume name (frei wählbarer Name)
	* claimName, verweisen auf mealia-storage.yaml
*  volumeMounts ist auf Container Ebene und mountet unser Volume in den Container

```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: mealie
  name: mealie
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
        - image: ghcr.io/mealie-recipes/mealie:v1.3.2
          name: mealie
          ports:
            - containerPort: 9000
          volumeMounts:
            - mountPath: /app/data
              name: mealie-data  
      volumes:
        - name: mealie-data
          persistentVolumeClaim:
            claimName: mealie-data      

```

```bash
# Volume in yaml hinzugefügt, deployment aktualiseren
k apply -f mealie-deployment
```


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
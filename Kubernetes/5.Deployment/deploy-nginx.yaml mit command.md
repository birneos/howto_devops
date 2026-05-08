Beispiel Datei für ein einfaches Deployment mit RollingUpdate Strategie

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



- Documentation im Mealie git Repo ansehen
- https://github.com/mealie-recipes/mealie/releases
- Namespace für die App erstellen
	```
	#erzeugt Namespace und zeig die yaml an
	k create namespace mealie -o yaml 
	```
 

### Deployment für die mealie-app erzeugen
```
k create deployment mealie --image=nginx --dry-run=client -o yaml > mealie-deployment.yaml
```

- Ergänze die yaml um  ```namespace: mealie ```
- replicas lassen wir auf 1 Instanz, für kleine Projekte

### Mealie Images

Um das Image für Mealie zu laden lesen wir uns die Homepage von Mealie durch

Unter Installation-Checkup finden wir Hinweise wo die akutellen docker images liegen

https://docs.mealie.io/documentation/getting-started/installation/installation-checklist/

```

ghcr.io/mealie-recipes/mealie:nightly

ghcr.io/mealie-recipes/mealie:<version>

ghcr.io/mealie-recipes/mealie:latest

```

### YAML Anpassungen

#### Port freigeben

Wir exposen den Port des Mealie Containers für den Pod

```
	
      containers:
      - image: ghcr.io/mealie-recipes/mealie:v1.2.0
        name: mealie
        ports:
          - containerPort: 9000
	  - image: ghcr.io/mealie-recipes/mealie:v1.2.0
	    name: mealie
        ports:
         - containerPort: 9000

```

### Deployment anwenden

```
k apply -f deployment.yaml
```

#### Mealie Pod läuft, wir geben den Pod mal aus...

Wir sehen mealie und eine zufällige codierung, daran erkennen wir kubernetes ein Replicaset nutzt und wir keinen festen Podnamen verwendet.

```
k get pods

NAME                         READY   STATUS    RESTARTS   AGE
mealie-58d548ff48-r4mnh      1/1     Running   0          6m41s


k describe pod mealie-58d548ff48-r4mnh
```

```
# Hilfe zu Port Forwarding anschauen
k port-forward -h | less


k port-forward pods/mealie-58d548ff48-r4mnh 9000



# Unsere VM ist nicht lokal, SSH Tunnel zur VM aufbauen

1. Baue einen SSL Tunnel auf
ssh -L 9000:127.0.0.1:9000 user@ip_der_entfernten_VM

2. Rufe im Browser auf
   http://localhost:9000
```


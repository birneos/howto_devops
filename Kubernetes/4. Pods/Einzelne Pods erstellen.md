Was sind Pods?

https://kubernetes.io/docs/concepts/workloads/pods/

_Pods_ sind die kleinsten einsetzbaren Recheneinheiten, die Sie in Kubernetes erstellen und verwalten können.

Ein Pod ist vergleichbar mit einer Gruppe von Containern mit gemeinsam genutzten Namensräumen und gemeinsam genutzten Dateisystemvolumes.

Pods in einem Kubernetes-Cluster werden hauptsächlich auf zwei Arten verwendet:

- **Pods, die jeweils einen einzelnen Container ausführen** . Das Modell „ein Container pro Pod“ ist der häufigste Anwendungsfall von Kubernetes; in diesem Fall kann man sich einen Pod als Wrapper um einen einzelnen Container vorstellen; Kubernetes verwaltet Pods, anstatt die Container direkt zu verwalten.
    
- **Pods führen mehrere Container aus, die zusammenarbeiten müssen** . Ein Pod kann eine Anwendung kapseln, die aus [mehreren eng gekoppelten, gemeinsam genutzten Containern](https://kubernetes.io/docs/concepts/workloads/pods/#how-pods-manage-multiple-containers) besteht , die Ressourcen gemeinsam nutzen müssen. Diese Container bilden eine zusammenhängende Einheit.


➜  `~ k get pods --all-namespaces`

NAMESPACE     NAME                                      READY   STATUS      RESTARTS       AGE
kube-system   coredns-76c974cb66-jfjjg                  1/1     Running     8 (28h ago)    8d
kube-system   helm-install-traefik-9qmnd                0/1     Completed   2              8d
kube-system   helm-install-traefik-crd-gdbns            0/1     Completed   0              8d
kube-system   local-path-provisioner-8686667995-s2ns6   1/1     Running     8 (28h ago)    8d
kube-system   metrics-server-c8774f4f4-gcjl4            1/1     Running     8 (28h ago)    8d
kube-system   svclb-traefik-f57a1205-8c6d5              2/2     Running     16 (28h ago)   8d
kube-system   traefik-c5c8bf4ff-hbw89                   1/1     Running     8 (28h ago)    8d


#### Wir erstellen einen Pod mit einem nginx container und geben die yaml aus

`k run nginx-yaml --image=nginx --dry-run=client -o yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
labels:
run: nginx-yaml
name: nginx-yaml
spec:
containers:

- image: nginx
  name: nginx-yaml
  resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  status: {}
```
  

# Resource aus der neuen YAML erstellen und den POD dem Cluster zuweisen

`k create -f nginx.yaml`

`k apply -f nginx.yaml`


# YAML editieren und POD aktualiseren

  nano nginx.yaml....bspw. zusätzliches label eintragen

```

labels:

  meinlabel: daslabel

```

  

`k apply -f nginx.yaml`

  

# PODS Interkationen

`k get pods`

`k get pods -o wide`

`k describe pod nginx-mltest | less`

`k delete pod nginx-mltest`

  

## In den Container wechseln

`k exec -it nginx-docs -- /bin/bash`

`k exec -h | less`
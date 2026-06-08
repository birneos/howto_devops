
[Namensräume | Kubernetes](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)

In Kubernetes bieten _Namespaces_ einen Mechanismus, um Ressourcengruppen innerhalb eines einzigen Clusters zu isolieren. Namen von Ressourcen müssen innerhalb eines Namensraums eindeutig sein, aber nicht zwischen Namensräumen. Namespace-basiertes Scoping ist nur für Namespaces anwendbar [Objekte](https://kubernetes.io/docs/concepts/overview/working-with-objects/#kubernetes-objects) _(z. B. Einsätze, Dienste usw.)_ und nicht für clusterweite Objekte _(z. B. StorageClass, Nodes, PersistentVolumes usw.)_

Für die production cluster, sollte der **default** Namespace nicht genutzt werden.

Empfehlenswerte ist jede Anwendung bzw. Workload sollte seinen eigenen Namespace haben. (Empfehlung des Udemy Authors Mischa)

Es ist möglich eine rollenbasierte Zugriffskontrolle zu machen, das nicht jeder auf jeden Namespace zugreifen kann.

`k create <tab> <tab>`

 `k create`
`clusterrole          (Create a cluster role)`
`clusterrolebinding   (Create a cluster role binding for a particular cluster role)`
`configmap            (Create a config map from a local file, directory or literal value)`
`cronjob              (Create a cron job with the specified name)`
`deployment           (Create a deployment with the specified name)`
`ingress              (Create an ingress with the specified name)`
`job                  (Create a job with the specified name)`
`namespace            (Create a namespace with the specified name)`
`poddisruptionbudget  (Create a pod disruption budget with the specified name)`
`priorityclass        (Create a priority class with the specified name)`
`quota                (Create a quota with the specified name)`
`role                 (Create a role with single rule)`
`rolebinding          (Create a role binding for a particular role or cluster role)`
`secret               (Create a secret using a specified subcommand)`
`service              (Create a service using a specified subcommand)`
`serviceaccount       (Create a service account with the specified name)`
`token                (Request a service account token)`

### **Wir erstellen einen neuen Namespace**

Wenn wir einen Namespace erstellen ist wird diese im Cluster (etcd) gespeichert, es wird keine yaml angelegt, wir können diese aber anlegen oder später exportieren.

`k create namespace <name>`

`k create namespace mealie -o yaml --dry-run=client`   oder

`k create namespace mealie -o yaml --dry-run=client > mealie-namespace.yaml`  

## Namespace löschen

Wenn in dem Namespace noch Resources vorhanden sind (Pods etc), dann werden diese mit delete auch gelöscht

`k delete namespace <name>`

## Namespace exportieren der bereits im Cluster existiert

`k get namespace mealie -o yaml > mealie-namespace.yaml`
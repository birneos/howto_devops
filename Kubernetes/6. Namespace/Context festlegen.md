## Context

Der Context bildet unseren Cluster ab und wir können also mehrere Contexte haben
zwischen den wir wechseln können. Per default wird immer ein Context angesprochen.

Wenn wir in einem Projekt arbeiten können wir einen default context setzen

Wenn wir kubectl get pods aufrufen, werden per default alle pods des aktuellen
Contextes/Clusters angezeigt.

### Aktuellen Context (aktuelle Cluster) in dem wir uns bewegen

`k config get-contexts`
`k config set-context --current --namespace=mealie`
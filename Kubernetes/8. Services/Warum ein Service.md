
Stell dir vor du hast für unsere Mealie Anwendung 10 Replicas laufen die alle eine IP zugeteilt bekommen und wir skalieren die Anwendung herunter, der Service kümmert sich um das entfernen
zuvieler Pods, ansonsten müssten wir das alles manuell machen müssen.

Oder wir Updaten unser Deployment. Unser Rolling-Update erzeugt neue Pods mit dem neuen
Deployment und entfernt nach und nach die alten Pods, sorgt aber dafür das unsere Anwendung
erreichbar bleibt.


* Pods are ephremal. You should not expect a pod to have a long lifespan
* Pods are constantly changing and beeing moved across nodes
* How will the system keep track of the constantly changing IP Adresses


`kubectl get service`
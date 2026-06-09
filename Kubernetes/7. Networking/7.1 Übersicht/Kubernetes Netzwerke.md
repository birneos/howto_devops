
Networking geschieht in Kubernetes auf POD Ebene und wird durch das CNI Plugin implementiert. Und anstatt den Datenverkehr selbst an bestimmt Pods zu leiten, verwenden wir Services als einzigen Endpunkt.

* Kubernetes hat einen Pool an IP Adressen
* jeder Pod bekommt seine eigene IP Adresse
* by default ist im Cluster jeder Pod per IP Adresse erreichbar
* mit Network Policies  kann die Kommunikation zwischen Pods und Namesspaces eingeschränkt werden
	* bspw. könnte unser Mealie-Projekt einen Namesspace haben und eine Policy legt fest das PODS nur innerhalb des Namespace kommunizieren dürfen
* Standardmässig können alle Pods miteiander Namespace übergreifend mit einer kommunizieren
* wir können Pods als virtuelle Maschine betrachten ( mit mehreren Containern) und die Container kommunizieren untereinander über localhost
* Um Ports von aussen erreichen zu können http://localhost:9000 müssen wir den Port nach aussen weiterleiten
* Wie machen wir Pods im Netz verfügbar: mit Services

**Port Weiterleitung** 
`k port-forward -n mealie pods/mealie-d78ff6bdd-cd999 9000`

**Alle Namespaces und deren IPs anzeigen**
Die Pods ohne IP sind in der Regel Jobs, die nur einmal ausgeführt wurden und keine IP benötigen.

`k get pods --all-namespaces -o wide`

### Was ist notwendig in Kubernetes, um die PODS miteinander zu verbinden?

#### Container Networking Interface Plugin

Stellen wir uns eine NIC (Netzwerkkarte vor), nichts anderes stellt CNI zur Verfügung

* CNI-Plugin (Container Networking Interface Plugin)
		- Stellt die Netzwerkanbindung zwischen den Containern her
		- konfiguriert die Netzwerkschnittstellen in den Containern

Unter K3s ist *flannel* meist integriert, weitere wäre auch cilium und calico

#### Welches CNI Plugin nutzt K3s ?

`sudo tree /var/lib/rancher/k3s/agent/etc/`

#### Welches CNI Plugin nutzt Rancher Desktop ?
`rdctl -h` 
`rdctl shell bash`
`sudo tree /etc/cni`






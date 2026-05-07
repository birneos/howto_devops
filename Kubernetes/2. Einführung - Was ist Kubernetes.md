https://kubernetes.io

* wird als sehr komplex von den Leuten dargestellt, in gewisser Weise ist es da
*  der Trick ist sich auf das zu beschränken was man für seien Arbeit brauch
*  ein endloses Kaninchenloch und man kann jeden Tag etwas dazu lernen


Was war vor Kubernetes

* VMs wurden erstellt und darauf liefen Anwendungen, jede VM für sich
* 2013 kam Docker mit seiner Containisierung ( Cotainaisierung wurde in den 1970er schon bei Unix eingeführt)
	* Anfrage kommt rein --> Loadbalancer und der verteilt auf die Anfragen auf die VMs
	* auf einer VM liefen dann mehrere Docker-Container, soviele die VM mit eigenem Betriebssystem und RAM und CPU betreiben konnte
	* VM --> Anwendungen ( Frontend, Backend, Webserver)
	* Anfragen steigen, also eine weitere VM erstellen für gleiche Anwendungen, das konnte
	 man gut mit Ansible managen
	 * Wie VMs anpassen an Anforderungen ? -- >manuelles abschalten der VMs, um sie zu updaten war notwendig, jede VM für sich --> mühevolle Arbeit

-> Person -> Loadbalancer -> VM1-x -> Application

![[Pasted image 20260428084057.png|697]]


*  Kubernetes erleichert das alles deutlich
	"Kubernetes ist eine Gruppe von virtuellen Maschinen, die in der Lage sind, reibungslos miteinander zu kommunizieren und ihre Arbeitslast aufzuteilen  "

**Beispiel:**

**Say in yaml:** *Kubernetes i want 3 replica of this image: linked* -> Control Plane -> ... 

Kubernetes wird immer versuchen 3 Replicas zu erzeugen, er erkennt wenn eine abestürzt ist und erzeugt automatisch eine neue Instanz.

* Kubernetes skaliert den Cluster in der Cloud für uns nach unseren Wünschen
* Bsp wir betreiben eine Online-Zeitung, wir kennen die Spitzenzeiten
	* K reduziert die Nodes nach Bedarf und Auslastung


![[Pasted image 20260429122451.png]]


* Skalieren nach Bedarf - anhand der CPU Auslastung, Anzahl der Anfragen, anhand irgendwelche Metriken, um die Anforderungen abzudecken, reduziere Nodes und die Container oder erhöhe sie 
* Kubernetes ist also ein System das unsere Anweisungen befolgt, Rechenleistung intelligent verwalltet über mehrere Nodes 
* Grundstruktur des Kubernetes Cluster ist die Kontrollebene (Control-Plane), welche alle unsere Anfragen managt (handelt), wir haben die Worker Nodes und die Anwendungen die darauf laufen


![[Pasted image 20260429153643.png]]
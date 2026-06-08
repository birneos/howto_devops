
* Kubernetes hat einen Pool an IP Adressen
* jeder Pod bekommt seine eigene IP Adresse
* by default ist im Cluster jeder Pod per IP Adresse erreichbar
* mit Network Policies  kann die Kommunikation zwischen Pods und Namesspaces eingeschränkt werden
* wir können Pods als virtuelle Maschine betrachten ( mit mehreren Containern) und die Container kommunizieren untereinander über localhost
* Um Ports von aussen erreichen zu können http://localhost:9000 müssen wir den Port nach aussen weiterleiten

**Port Weiterleitung** 
k port-forward -n mealie pods/mealie-d78ff6bdd-cd999 9000


Was ist notwendig, um die PODS miteinander zu verbinden

* CNI-Plugin (Container Networking Interface Plugin)
		- Stellt die Netzwerkanbindung zwischen den Containern her
		- konfiguriert die Netzwerkschnittstellen in den Containern
		- 


```mermaid
%%{init:{
	 'themeVariables': {  
	'fontSize': '12px'  
	},  
	'flowchart': {  
	'useMaxWidth': false,  
	'nodeSpacing': 80,  
	'rankSpacing': 180 
	},
  "flowchart":{
	    "defaultRenderer":"elk"
  }
}}%%

flowchart LR

%% INGRESS
INGRESS --> IngressNum1[Networking on Pod Level]
INGRESS --> IngressLevel1[Each pod gets its own IP address]
INGRESS --> IngressPodItem1[By default, pods can connect to all pods on all nodes]

Main --> PODS

%% Kubernetes Networking
Main --> INGRESS[Ingress]

Main((Kubernetes Networking))
Main --> CNI[CNI Plugin]
Main --> Services

%%Container Networking Interface
CNI[CNI Plugin] --> Cni1[Provides network connectivity to containers]
CNI --> Cni2[Configures network interfaces in containers]
CNI --> Cni3[Assigns IP addresses and sets up routes]
Cni3 --> IPTABLES[IPTables on nodes]
CNI --> Cni4[Implemented by CNI Plugins]

Cni4 --> Cilium
Cni4 --> Calico
Cni4 --> Flannel


%% PODS
PODS --> PodItemNetwork[Networking on Pod Level]
PODS --> PodItemIP[Each pod gets its own IP address]
PODS --> PodItemAll[By default, pods can connect to all pods on all nodes]
PODS --> PodItemContainer[Containers in pods can communicate with each other throug localhost]

%% Styling
classDef infra fill:#dbeafe,stroke:#1d4ed8,color:#111;
classDef service fill:#dcfce7,stroke:#15803d,color:#111;
classDef external fill:#fef3c7,stroke:#d97706,color:#111;

class PODS,CNI,INGRESS infra;
class API,REP,REG,IDX,SEARCH,AUTH,WEB,VIEWER,TENANT,USERSVC,REND,RENDNET service;
class KROSS,GVD,GDA,KAFKA,GECO,DATASIGN,BUSTRA external;

```




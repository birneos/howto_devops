
1. WSL2 muss installiert sein
2. Ranches Desktop installieren
3. brew als Packetmanager bietet unter Ubuntu (WSL) oft mehr Pakete --> Empfehlung
	
4. Commandzeilentool kubectl und k9s installieren
	* `brew install kubectl k9s`
 5. Tuxmux und VIM installieren
	 * `sudo apt install tumux vim`

6. Testen ob kubernetes cluster in Ranches angesprochen werden kann
	* WSL: kubectl get pods

		erwartete Ausgabe
		`➜  ~ kubectl get pods`
		`No resources found in default namespace.`

		falls soetwas in der Art kommt, muss die config noch WSL bekannt gemacht werden
		`➜  ~ kubectl get pods`
			`E0424 12:51:17.626889   16720 memcache.go:265] "Unhandled Error" err="couldn't get current server API group list: Get \"http://localhost:8080/api?timeout=32s\": dial tcp 127.0.0.1:8080: connect: connection refused"`
			`E0424 12:51:17.627250   16720 memcache.go:265] "Unhandled Error" err="couldn't get current server API group list: Get \"http://localhost:8080/api?timeout=32s\": dial tcp 127.0.0.1:8080: connect: connection refused"`
			
		**Lösung:**
			**Make Kubeconfig available in WSL**

			WSL couldnt see the Windows-Kubeconfig.

			Windows:
			
			1. C:\Users\<YOUR_USER>\.kube\config
			
			In WSL, you need to make them available, for example:
			
			2. mkdir -p ~/.kube
			3. cp /mnt/c/Users/<YOUR_USER>/.kube/config ~/.kube/config

7.  Testen ob kubectl suggestion funktion, bsp. kubectl r <tab> sollte Empfehlungen zeigen

			Falls nicht, dann folgendes in WSL eingeben
				`autoload -Uz compinit`
				`compinit`
			 oder
				**These can be added to `~/.zshrc` as well for future shell sessions.**
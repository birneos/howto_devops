### Use `<Tab><Tab>` for suggestions

kubectl describe pod elasticsearch-0 -n infrastructure
kubectl config get-context
kubectl config get-contexts
kubectl config set-context


#### Hilfe ansehen, Ausgabe an Texteditor umleiten
k create deployment -h | vim -
k create deployment -h | less

#### watch ist Linux , um anzugeben gib alle Sekunde folgenden Befehl aus
`watch -n 1 "kubectl get pods"`

#### tmux
`<STRG> + b   c   neues Fenster`
`<STRG> + b   "    2tes Fenster horizontal`
`<STRG> + b   %   2tes Fenster vertikal`
`<STRG> + b   o    Toggeln zwischen Fenstern`


#### Fuzzy Finder

`dnf install fzf`


```bash
#adde das zu deiner zsh shell 
eval "$(fzf --zsh)"

# für die shell einmal aus
source ~/.zshrc 
``` 


# OSCP PWK
Trucs et astuces pour le PWK de l'OSCP et pour la suite

## L'organisation
Pour faire la formation et passer la certification PWK de l'OSCP, nous devons utiliser la distribution Kali Linux. 
Personnellement, ce n'est pas un problème car je l'utilise déjà régulièrement.

### Workspaces
Personnellement, j'utilise le premier workspace pour les outils de pentest. J'utilise le second pour tout ce qui relève de la configuration avec des terminaux pour le vpn, le client à distance pour windows7, un firefox pour revert les machines, etc.
Ainsi, je ne suis pas polué par la configuration lorsque je travaille.

## Découvrir le réseau
### Nmap
Nous pouvons utiliser nmap, qui est un outil rapide et puissant, pour découvrir le réseau. Pour ce faire, nous pouvons faire ceci :

`user@kali:# nmap -v -sV 10.11.1.0-254 -oG network.txt`

Et quand c'est finit, nous pouvons filtrer les résultats ainsi : 

`user@kali:# grep Up network.txt | cut -d " " -f 2 > network_filtered.txt`

Nous pouvons répéter ça pour les différents ports qui nous intéressent.

### Meterpreter
Un autre moyen de scanner le réseau est d'utiliser meterpreter. L'avantage de cet outil est qu'il embarque une base de données. Nous pouvons requêter cette base ainfi de trouver facilement nos résultat par la suite.

`user@kali:# msfdb init` ou `start` si nous l'avons déjà initialisée.

`user@kali:# msfconsole`

`msf > db_nmap -v -T 5 10.11.1.0-254` : attention, -T 5 est la vitesse de parcours la plus rapide. C'est un super moyen d'avoir rapidement des résultats, mais également afin d'être vite détecter sur un réseau

`msf > services -p 161` pour avoir la liste des serveurs snmp qui ont été découvert lors de l'analyse. Mais, nous pouvons aussi les rechercher ainsi : `msf > services -S snmp`. A mon avis, l'avantage est que si un service a été trouvé sur un autre port, nous pourrons le voir à travers cette recherche.

## Trouver des vulnérabilités
### Nmap
Et oui, nmap peut être utile pour chercher des vulnerabilités sur le réseau. Pour ce faire, il suffit de lancer une commande telque : 

`user@kali: # namp --script=smb-vuln-ms07-029 XXX.XXX.XXX.XXX`

Normalement, les scripts nmap se trouvent `/usr/share/nmap/scripts/`. Donc pour connaitre, les différents scripts de vulnérabilité, il suffit d'utiliser la commande suivante : 

`ls /usr/share/nmap/scripts/*vuln*`

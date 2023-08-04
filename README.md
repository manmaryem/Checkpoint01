# SB - TSSSR | Checkpoint01
## 1.1 Éléments du réseau - QUIZZ
![image](https://github.com/manmaryem/Checkpoint01/assets/137881827/07cfb095-1a71-40d9-aa47-6f704aa50a0b)


- **Pour le matériel B, connais-tu ses différences avec A ?**
- Le materiel **A** est un switch qui connect les différentes adresses MAC, ou tout appareils sur un même réseau local (LAN). Le materiiel **B** est un PfSense qui peut connecter des appareils à différents réseaux locaux, ainsi que des réseaux locaux à des réseaux Internet. il est également capable d'appliquer des règles de pare-feu aux paquets de données, ce qui peut aider à protéger contre les attaques.
  
- *****Que représente em0, em1, em2 ?*****
- em0, em1 et em2 représentent les interfaces réseau. Em0 est la première interface réseau, em1 est la deuxième interface réseau, em2 est la troisième et ainsi de suite.
  
- *****Que signifie /26 ?*****
- Le /26 signifie un masque de sous-réseau avec 26 bits. Cela correspond à un sous-réseau de 64 adresses IP, avec 62 adresses utilisables et 2 adresses réservées.
  
- *****Dans le vocabulaire IP, qu'est-ce que Ubuntu-1, Ubuntu-2, ... ?*****
- se sont des noms d'hôte, donc chaque machine comporte son propre nom et son propre @IP afin de les Différencier.
  
- *****De même, qu'est-ce que A et B ?*****
- materiel (A) Est un switch son périphérique est connecté sur le réseau local (LAN) et qui permet à son tour de connecter ubuntu 2 au périphéirique de pfsens. Le switch (A) utilise l’adresses MAC 00:50:79:66:68:01 pour le routage et le partage des informatons. Le materiel **B** est un PfSense un routeur de pare-feu de couche 3. il peut connecter des appareils à différents réseaux locaux, ainsi que des réseaux locaux à des réseaux Internet.
  
- *****Peut-on considérer que Ubuntu-2 est connecté directement à em1 de l'équipement pfSense ?*****
- Non, Ubuntu-2 n'est pas connecté directement à em1 de l'équipement pfSense. Ubuntu-2 est connecté au switch, puis au pfSense, puis à Internet. Le switch est situé entre Ubuntu-2 et em1.
  
# 1.2 Etude théorique

![image](https://github.com/manmaryem/Checkpoint01/assets/137881827/311e667a-ba46-4b2b-878b-955e18ee19bc)

Par rapport aux adresses IP de chaque PC :

*Calcul pour les deux réseaux :*

- *****L'adresse de diffusion*****
- Ubuntu 1: 10.0.1.0/26 : 10.0.1.63
- Ubuntu 2 : 10.10.0.0/24 : 10.10.0.255

- *****La plage d'adresses disponible*****
- Ubuntu 1: 10.0.1.0/26 : 10.0.1.2 à 10.0.1.62
- Ubuntu 2 : 10.10.0.0/24 : 10.10.0.2 à 10.10.0.254
- *****La machine Ubutun-1 et Ubutun-2 peuvent elle communiquer entre elle ? Explique la raison.*****
- Oui, les machines Ubuntu-1 et Ubuntu-2 peuvent communiquer entre elles. car, les deux machines sont sur le même réseau et ont des adresses IP qui se trouvent dans la même plage d'adresses disponibles.
- *****De même, quelles machines vont pouvoir sortir du réseau ?*****
- Les machines Ubuntu-1, Ubuntu-2 pourront sortir du réseau. puisque les deux machines sont sur le même réseau et ont des adresses IP qui se trouvent dans la même plage d'adresses disponibles.
- *****On veut passer les adresses IP des machines en dynamique pour qu'elles puissent toutes communiquer entre-elles. Doit-on ajouter des éléments au schéma pour que cela soit possible ? Deux situations sont possible.*****
- (\ il faut ajouter un serveur DHCP au réseau pour passer les adresses IP des machines en dynamique. 1ère possibilité, soit on ajoute un serveur DHCP distinct à chaque réseau. Dans ce cas, le serveur DHCP sur le réseau 10.0.1.0/26 fournira une adresses IP à la machine sur ce réseau, et un serveur DHCP sur le réseau 10.10.0.0/24 qui fournira une adresse IP à la machine sur ce réseau. 2ème posibilité dans notre schéma, c'est le pfSense est qui est configuré en tant que serveur DHCP pour les deux réseaux. Les machines Ubuntu-1 et Ubuntu-2 ont leurs adresses IP, leurs masques de sous-réseau et leurs passerelles par défaut de pfSense*)
  
# 1.3 Analyse de trames #
![image](https://github.com/manmaryem/Checkpoint01/assets/137881827/848c87ea-b5c2-4c4b-9a3a-1fa244e7368e)

## Fichier 1 :

*****Dans cette trame, qui initialise la communication ?*****
- c'est l'hôte qui émet un paquet ARP 10.10.4.1. donc ce paquet ARP demande à l'hôte qui possède l'adresse IP 10.10.4.2 de lui envoyer son adresse MAC afin qu'ls puissent communiquer ensemble.

*****Est-ce que cette communication a réussi ? Si oui, indique entre quels matériel, si non indique pourquoi cela n'a pas fonctionné.*****
- Les deux matériels qui communiquent sont ![image](https://github.com/manmaryem/Checkpoint01/assets/137881827/e32d20a8-468f-464c-8eb5-c7759b5f2ac5)

Donc, Oui, la communication a réussi. Le paquet ARP a été répondu et l'adresse MAC 00:50:79:66:68:03 de l'hôte 10.10.4.2 a été trouvée.

 ![image](https://github.com/manmaryem/Checkpoint01/assets/137881827/97f7caa8-c96f-428b-9187-a053871c2ad8)

*****A qui correspond le request et le reply dans toute la trame ?*****
- il s'agit ici de deux hôtes qui se font pinger mutuellement. Le protocole ICMP est utilisé pour tester la connectivité entre deux hôtes. 

*****Quels ont été les rôles des matériels A et B ?*****
- Le materiel "A" a envoyer un paquet ARP donc une **request** pour demander l'adresse MAC du matériel "B". Le matériel "B" a répondu donc un **reply** par paquet ARP en fournissant son adresse MAC au matériel A. c'est de là que les deux peuvent communiquer ensemble.

  ## Fichier 2 :

*****Dans cette trame, qui initialise la communication ?*****
- L'hôte 10.10.80.3 qui initialise la communication en envoyant des requêtes ICMP (ping) à l'hôte 10.11.80.2. ![image](https://github.com/manmaryem/Checkpoint01/assets/137881827/89993b5b-4c44-43d2-aeca-06f421e81eaf)
  
*****Est-ce que cette communication a réussi ? Si oui, indique entre quels matériel, si non indique pourquoi cela n'a pas fonctionné.*****
- Non, la communication n'a pas réussi. Les paquets ICMP (ping) n'ont donnée aucune réponse, l'hôte 10.11.80.2 n'est pas accessible ![image](https://github.com/manmaryem/Checkpoint01/assets/137881827/cdeea439-4f8a-452d-ba18-993b16b648f9)

*****Quels ont été les rôles des matériels A et B ?*****
- L'hôte (**A**) 10.10.80.3 envoie les requêtes ICMP (ping). Et le L'hôte (**B**) 10.11.80.2 est pingé, mais ne répond pas aux requêtes.

## Fichier 3 :

*****Dans cette trame, qui initialise la communication ?*****
- L'hôte 10.10.80.3 qui initialise la communication il a envoyé un paquet ARP à l'adresse broadcast. il a demandé l'adresse MAC de l'hôte qui possède l'adresse IP 10.10.255.254. ![image](https://github.com/manmaryem/Checkpoint01/assets/137881827/099ee745-afc9-4be3-ba13-bc86be04f090)

*****Est-ce que cette communication a réussi ? Si oui, indique entre quels matériel, si non indique pourquoi cela n'a pas fonctionné.*****
- Non, la communication n'a pas réussi. Les paquets ICMP (ping) ont données aucune réponse le serveur DNS dns.google n'est pas accessible.![image](https://github.com/manmaryem/Checkpoint01/assets/137881827/57a2d263-192a-42d0-82be-5ba1ece43548)

*****Quels ont été les rôles des matériels A et B ?*****
- L'hôte 10.10.80.3 envoie les requêtes ICMP (ping) vers Le serveur DNS dns.google, ce serveur qui est pingé, ne répond pas aux requêtes.
  
*****Où vois-tu les différents protocoles encapsulés ?*****
- les adresse IP mais je ne suis pas sûr
  
  # Exercice 2 - scripting
  
  
  

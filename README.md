# PostgreSQL-en-container
PostgreSQL en container : découverte de l'opérateur Kubernetes CloudNativePG.

Atelier du 14 janvier 2025 - PGSession 17

decouverte et prise en main de l'operatuer avec PostgreSQL

deploiment d'instances PostgreSQL

test et decouverte

---------------
quelques mots sur postgresql dans Kubernetes

l'operateur CloudNativePG

-----

kubernetes : plateforme de deploi de conteneur open source, init par google, il existent plusieurs distribs

deploie applicatifs, auto-scalling redeploi auto, load

habituellement du stateless (d'où l'apparition des statefulset : objet qui permet de garde les donnees)

---------------------

ce n'est pas un effet de mode !!!

----
deploiyer 
- necessite de choisir une image docket contenant PostgreSQL
- dockerhub, image maison
vient ensuite le deploiement
- manuel
- statefulsethelm cahrt
- opearateur (leurs propres images)

---------operateurrs postgresql :

-extension des fonctionnalites et des ressources de Kubernetes
- plusieiurs operatuers pour PostgreSQL
- maturite variable en fonction des projets

--------- operateur : CloudNativePG
projet open source : depuis la liberation du code en 2022, de plus en plus utilité
grand attrait github
tentative d'incubation
de plus en plus cites dans kubecon, pgconfeu

--------------------

ce que permet CloudNativePG :

deploiement des instances postgres facilite
mise en place de replication auto
sauvegarde pitr, planifies
bascule (auto ou manuelle)
haute disponibilte
hibernation, fencing
plugin kubectl

------------

tout n'est pas simple :

- connaissances kube (meme en tant que dba)
- connaissances postgres (meme en tant qu'admin K8S) 
- changements d'habi et d'outils : plus de ssh sur la machine ou se trouve l'instance, configuration dans les ressources kube
- couches d'absractions supplementaires (debug plus long plus compliqué, avoir les bons outils)
- jongler avec les versions : kubernetes : 3 supportees, cloudnativepg : 2 supportees, postgresql : 5 supportees

------
travaux pratiques :

https://raw.githubusercontent.com/cloudnative-pg/cloudnative-pg/release-1.24/releases/cnpg-1.24.0.yaml

l'element important de cet fichier yaml est : image: ghcr.io/cloudnative-pg/cloudnative-pg:1.24.0

la commande : kubectl get pods -n cnpg-system => permet de savoir que l'operatuer est installe (en running)


---------------
Retrouver la liste des nouvelles ressources créées :

kubectl api-resources --api-group postgresql.cnpg.io

------------------

il faut un objet !: secret pour sauvegarder le mot de passe de la bdd

par defaut , le fichier yaml cree une bdd app. mais on peut la nommer dans le fichier yaml (voir doc).

----------
la commande : kubectl get svc  => est tres utile pour lister les services

---------------

replication slot = pour securiser la republication

----------
les operations de mises à jour : dans la def yaml , je ne veux plus etre le nom de l'image...
----------
barman (utilise pgbabase backup) est l'equivalent de pgbackrest, pour sauvegarder sur du s3 (amazon, minae..)

barman est l'outil pour la sauvegarde dans CloudNativePG

----

à retenir : l'operatuer gere les instances...il faut aussi le mettre à jour sans parler de la mise jour PostgreSQL
------

pour du postgresql dans la prod, il faut utuiliser un operatuer...sinon c'est du suicide

---------------------

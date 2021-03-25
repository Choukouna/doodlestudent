#Compte rendu du projet *doodlestudent*

##Etapes de déploiement de l'application

Builder l'image du backend
-----
<p>
    Le fichier Dockerfile.jvm du repertoire *src/main/docker/* permet de builder et packager le backend <br>
    **Commande** : docker build -f src/main/docker/Dockerfile.jvm -t backend_img:1.0
</p>

Builder l'image du front
-----
<p>
    Le front étant une app angular, on doit compiler ses sources avec la cli angular (ng-build et tout)
    Nous utilisons un dockerfile à cet effet dans le repertoire *front/Dockerfile*<br>
    Ainsi à partir d'une image de node, on compile les sources angular et déposons ces sources dans le répertoire dist du container <br>
    Par la suite l'on reprend une image de serveur web notamment *bunkerized-nginx* sur laquelle on copie nos sources compilées dans le repertoire /www de celui-ci<br>
    **Commande** : docker build -f front/Dockerfile -t frontend_img:1.0 . 
</p>

Lancer la stack de container
-----
<p>
    Créer un docker-compose pour lancer les différentes images créées (backend, front, db, smtp, etherpad)<br>
    Voir fichier docker-compose.yml du repertoire *api*;<br>
    **Commande**: docker-compose up --build
</p>

Configurations
----
Sur la machine local
-----
Mettre à jour le fichier de configuration du backend *application.yml*
Ainsi les requetes sur /api sont redirigées sur le backend ici par 
    if($host = localhost) {}

Sur la vm
-----
Mettre à jour le fichier de configuration du backend *application.yml*
Ainsi les requetes sur /api sont redirigées sur le backend ici par
    if($host = rlok.diverse-team.fr) {}

##Etat du projet 
<p>
    Avec les dockerfiles et docker-compose actuels, nous arrivons à lancer les services **back, front, db et ehterpad** en local <br>
    Cependant une fois sur le serveur nous eûmes certaines difficultés. En effet, les services ont été deployées à l'identique sur la vm (en http)
    correctement un première fois. De là nous avons changer les configurations du bunkerized-nginx pour passer en HTTPS cependant
    des certificats n'étaient pas trouvés pour certains serveurs comme pour le *pad.rlok.diverse-team.fr* <br>
    Après des tentatives de vaines de correction, nous n'avons pas réussi à passer en HTTPS <br>
    Nous sommes revenus donc à la configuration pour du HTTP uniquement mais on a une erreur **403 Forbidden** qu'on avait pas !<br>
    Neanmoins cela fonctionne sur la machine locale :-)
</p>

##Conclusion
<ul>
    <li>Ce projet nous aura permis de manipuler et comprendre l'utilité d'images docker et de docker-compose pour 
    la gestion d'une stack de container. </li>
    <li>Il nous a permis de comprendre le principe d'utilisation d'une image docker (accès docker hub, rechercher les confs de base, les variables d'environnement à utiliser)</li>
    <li>Neanmoins les problèmes rencontrés en passant de la machine locale à la vm nous ont laissé quelques doutes sur l'exécution identique d'un container d'une machine à une autre</li>
</ul>
********
Git flow
********

.. sidebar:: Git flow

    .. image:: ../_static/git-flow-schema.png
        :align: right
        :alt:  gitflow schema

Initialisation
**************
Crée la branche develop et checkout dessus

::
       $ git fow init

Les branches
************

* **master**: la branch principale
* **develop**: c'est la copie de la branch master au départ. c'est la branche de développement. C’est ici qu’on va tirer les branches pour travailler sur les features/releases, elle correspond à l’environnement de développement, on y prépare les changements en vue de la prochaine release dans master

.. warning:: on ne travaille pas sur master et develop : pas de commit et push !

* **feature/<nom de la feature>**: tirée de la branch develop, elle permet de travailler sur de nouvelles fonctionnalités. une fois terminée , on la merge a develop.

* **release/<nom de la release( version n°)>**: tirée de la bracnh develop . Elle regroupe toutes les features ajoutées depuis la derniere release. on fait les derniere vérification dessus read me, correction de bug et ensuite on la merge dur develop et master avec un changement de tag !

* **hotfix/<xxx>**: on l'utilise en cas d'extrème urgence : dédié a la correction des bugs de la production master. on la tire de cette dernière et une fois le bug corrigé on merge sur develop et master.

Gestion d'une feature
**********************
Création
========
Pour créer une feature

::
   $ git flow feature start <nom de la feature>

publication
===========
Pour publier sur un serveur distant notre feature:

::
   $ git flow feature publish < nom de la feature>

fin de la feature
=================
Une fois qu’on a terminé notre feature, il suffit de taper la commande 

::
   $ git flow feature finish <nom de la feature>
    
Git Flow va donc :

* **fusionner la branche feature/main avec develop**
* **supprimer la branche feature/main en local et sur notre serveur**
* **nous balancer sur la branche develop.**

Gestion d'une realease
**********************
Création
========
Pour créer une realease

::
   $ git flow release start <v0.1>

publication
===========
Pour publier sur un serveur distant notre release:

::
   $ git flow release publish < v0.1>

fin de la release
=================
Une fois qu’on a terminé notre release, il suffit de taper la commande 

::
   $ git flow release finish <V0.1>
    
Git Flow va donc :

* **merger notre release avec la master**
* **tagger notre realease**
* **merger la release avec develop**
* **supprimer la branche release en local et sur notre serveur**
* **nous balancer sur la branche develop.**

reste à publier les modifications de version:

:: 
    git push --tags

publier les modification sur la master

::
    git push origin master

Gestion d'une hotfix
**********************
Création
========
Pour créer une hotfix

::
   $ git flow hotfix start bug

fin de la hotfix
=================
Une fois qu’on a terminé notre hotfix, il suffit de taper la commande 

::
   $ git flow hotfix finish bug
    
Git Flow va donc :

* **merger notre hotfix avec la master**
* **tagger notre hotfix**
* **merger la hotfix avec develop**
* **supprimer la branche hotfix en local et sur notre serveur**
* **nous balancer sur la branche develop.**



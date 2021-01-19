************
Git & Github
************

Commandes de Git
****************

Initialisation
==============
Initialisation git
------------------

::

    $ git init
    
    $ git flow init     // pour initialiser avec gitflow

  
Config dépôts
-------------


::

    $ git remove -v     // liste tous les repertoires distants existants
    
    $ git remote add < nomDepotLocal > < url: https://mondepotdistant >     //nomDepotLocal pointera sur mondepotdistant
    
    $ git clone <monDepotDistant>   // pour cloner un depot distant en local


.. sidebar:: Avec git remote

    * origin    =    le depot local forké sur Github 
    * upstream  =    le depot distant d'origine

Liste config git
----------------

::

    $ git config --list

Commandes principales
=====================

Branches
--------

::

    $ git branch    //liste toutes les branches existantes

    $ git branch <newBranch>    //crée une nouvelle branches

    $ git branch -d <branch>    //supprime la branches
    
    $ git branch -D <branch>    //supprime en force la branche en force même si des modifs dessus et sans Commit

    $ git checkout <branch>     //se positionner sur la branch


Staging
-------

::

    $ git status    //affiche l'état du staging dans la branch 

    $ git add <fichier>     //ajout d'un fichier dans le Staging

Commit
------

::

    $ git commit -m "message du commit"     // enregistre un commit et avance la HEAD a ce dernier


History
-------

::

    $ git log [--oneline] [--graph] //liste des commits avec toutes les infos sur une branche

        options:
            --oneline   donne juste la liste de tous les commits (une ligne pour chaque commit)
            --graph     crée une sorte de graph , permet de voir la création d'autre branche

    $ git reflog    // liste toutes les actions faites (commit, checkout,merge...) sur toutes les branches 







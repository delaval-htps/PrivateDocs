***
SQL
***

One to One
**********

La relation 1:1 est semblable a celle de 1:Many , elle peut s'effectuer de deux maniéres:

* Créer un FK avec le PK de chaque table dans les 2 tables: cela consiste a créer deux 1:M entre les deux tables

* Créer un Fk unique dans une des tables mais avec son id (pointant sur l'id de la seconde table)


  .. code-block:: sql

    DROP DATABASE IF EXISTS test;
    CREATE DATABASE test CHARSET = utf8 COLLATE = utf8_general_ci;
    USE test;

    CREATE TABLE users(
    id INT NOT NULL AUTO_INCREMENT,
    user_name VARCHAR(45) NOT NULL,
    PRIMARY KEY(id)
    ) ENGINE = InnoDB DEFAULT CHARSET = utf8;

    CREATE TABLE accounts(
    id INT NOT NULL AUTO_INCREMENT,
    account_name VARCHAR(45) NOT NULL,
    PRIMARY KEY(id),
    FOREIGN KEY(id) REFERENCES users(id)
    ) ENGINE = InnoDB DEFAULT CHARSET = utf8;

Transaction
***********

Definition
++++++++++

Les transactions permettent de regrouper plusieurs requetes.

.. note:: 
  **Example de transaction :**
  
  un virement depuis un compte a un autre implique deux requetes : une pour modifier le premier compte (soustraire le montant) une seconde pour modifier le second compte ( ajouter le montant du virement)

  On doit donc faire une transaction pour effectuer ce virement si l'une des requetes n'aboutie pas alors on pourra ne pas valider le virement et revenir en arriere et annuler l'autre requete


.. warning:: 
  
  Une Transaction ne peut etre effectuée que dans une base transactionnelle, qui pour Mysql est defini par le moteur de stockage:

  * MyIsam est non transactionnelles car elle ne supporte pas les FK;
  * InnoDB est transactionnelle donc supportent les transactions:


Utilisation
+++++++++++

Par Défaut, Mysql est en mode 'autocommit', ie: chaque requete est directement commitée sans retour en arriéré  possible.

Pour quitter ce mode on peut faire la commande suivante:

.. code-block:: SQL
  
  SET autocommit=0;


Il faudra ensuite commiter ou revenir en arriére manuellement pour chaque requete...

Pour démarrer une transaction on utilise la commande suivante:

.. code-block:: SQL

  START TRANSACTION;

Pour valider et/ou annuler une transaction on utilisera les commandes suivantes:

.. code-block:: SQL
  
  COMMIT;
  ROLLBACK;



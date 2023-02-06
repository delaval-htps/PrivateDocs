*************************
datasource-initialisation
*************************

Initialiser la BDD avec scripts SQL
***********************************

Il suffit de mettre directement dans /src/main/resources les fichiers:

* *schema.sql* : pour créer la database et les tables
* *data.sql* : pour inserer des données.

SpringBoot lira automatiquement au démarrage ces fichiers pour créer et persister les données.

.. warning:: il faut penser a désactiver l' option **spring.jpa.hibernate.dll-auto= none**  dans application.properties pour ne pas laisser hibernate creér lui même la base de donnée en focntion des entities.


.. list-table:: **option spring.jpa.hibernate.dll-auto**
    :widths: 30 100
    :header-rows: 1 

    * - valeur
      - definition
    * - **none** 
      - hibernate ne créera aucune base de donnée 
    * - **create-drop**
      - hibernate va créer la base de donnée et les tables en fonction des entités et la dropera a la fin de l'application
    * - **create**
      - hibernate va créer la database et les tables en fonction des entités et detruira les données 
    * - **update**
      - hibernate mettra a jour la database et les tables existantes
    * - **validate**
      - hibernate validera la database mais ne fera pas de changements 










   
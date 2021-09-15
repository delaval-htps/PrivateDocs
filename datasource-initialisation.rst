*************************
datasource-initialisation
*************************

Initialiser la BDD avec scripts SQL
***********************************

Il suffit de mettre directement dans /src/main/resources les fichiers:

* *schema.sql* : pour créer la database et les tables
* *data.sql* : pour inserer des données.

SpringBoot lira automatiquement au démarrage ces fichiers pour créer et persister les données.

.. note:: il faut penser a désactiver l' option **spring.jpa.hibernate.dll-auto= none**  dans application.properties pour ne pas laisser hibernate creér lui même la base de donnée.







   
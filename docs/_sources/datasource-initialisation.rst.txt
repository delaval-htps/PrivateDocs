*************************
datasource-initialisation
*************************

Initialiser une base de données à l'aide de scripts SQL de base
***************************************************************

il suffit de mettre directement dans /src/main/resources les fichiers:

* schema.sql : pour créer la database et les tables
* data.sql : pour inserer des données.

SpringBoot lira automatiquement au démarrage ces fichiers pour créer et persister les données.

.. warning:: il faut penser a désactiver l' option **spring.jpa.hibernate.dll-auto**  en la mettant à **none** dans application.properties pour ne pas laisser hibernate creér lui meme la base de donnée sinon il utilise les entitys fournis pour créer la bdd.


   
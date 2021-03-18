***********************
Création d'une REST API
***********************

Creation Projet
***************

SpringBoot Starter
++++++++++++++++++
Uliliser Springboot initializer ou springboot Starter dans eclipse.

Renseigner les différents stater à utiliser.

N epas oublier d'importer le projet si uitlisation de SpringBoot initializer.

@SpringBootApplication
++++++++++++++++++++++

Annotation utilisée pour permettre au container de SpringBoot de manipuler les classes. Elle se compose de trois Annotations:

* **@SpringBootConfiguration**: la classe sera utiliser pour la configuration.
* **@EnableAutoConfiguration**: active la fonctionnalité d'auto configuration de SpringBoot.
* **@ComponentScan**: active le scanning des classes et packages dans lesquels il trouvera les beans.

Class Application 
=================
Class de démarrage du projet qui contient la **methode main():**

Cette dernière appele **SpringApplication.run(<nom de la classe d'application>.class,args);**

C'est cette methode run qui lancera l'application.

Ajout de code dans la classe Application
========================================

Pour lancer du code depuis cette class Application:
* Ne pas toucher a la methode **SpringApplication.run(<nom de la classe d'application>.class,args);**
* faire implementer la class Application a **l'interface CommandeLineRunner**
* **overrider la methode run(String ... args)** de cette interface pour y mettre notre code

Au démarrage, SpringBoot exécutera en premier la méthode run() overridée 

application.properties
======================
Fichier de configuration ou l'on mets les parametres de configuration(propriétés):

exemple:

* spring.application.name : permet de donner un nom à notre application Spring Boot.
* logging.level.[package] : permet d’indiquer le log level pour un package donné.

pour d'autre propriétés voir <https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html>

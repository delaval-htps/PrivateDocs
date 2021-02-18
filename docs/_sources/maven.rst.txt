*****
Maven
*****

Build lifecycles
********************

.. seealso:: ref doc technique de Maven

    <http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#lifecycle-reference>


les cycles de vie de construction sont aux nombres de 3:

* **default**:  gère le déployement du projet
* **clean** :   gére le nettoyage du projet
* **site** :    gére la creation de la documentation du site

Phases of lifecycles
********************

un cycle de vie comporte des phases de construction:

* **validate**:  valide le projet s'il est correct
* **compile** :   compile le code source
* **test** :    effectue les test unitaires 
* **package**:  crée le un package(conteneur, ex: .jar,.war...) avec le code compilé
* **verify**:   lance tous les test ainsi que les test d'intégration.
* **install**:  installe le package dans le repertoire local, peut etre utile pour l'utiliser dans d'autre projet
* **deploy**:   copie le package dans le repository distant pour etre partagé

Elles s'effectuent dans l'ordre pour accomplir le lifecycle default.
Pour les lancer on utilise la commande :
::

    $ mvn <nom de la phase>

.. seealso:: ci dessous la liste complete  des phases pour chaque lifecycle:

 <http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#lifecycle-reference>


Phases non CLI
**************
il existe des phases qui ne sont pas directement appélée par ligne de commande: **"pre-, post-, process-"** et **"integration-test"**.
Elles permettent de séquencer le build mais ne fournissent pas de resultat nécessaire a sa construction. 

Les outils tels que jacoco, failsafe, surefire et les conteneurs docker, Tomcat... lient des goals à ces phases pour générer des rapports. 

Goals plugin
************
un plugin comporte un ou plusieurs goal qui vont lui permettre de s'exécuter ou pas dans la phase demandée.

un goal non lié a une phase peut etre executer seul par l'appel suivant:
::

    $ mvn <dependency>:<goal>


on peut rajouter des phases en plus qui seront indépendantes de l'appel du goal du plugin.

Example:
::

    $ mvn clean sonar:sonar 


si on détermine un goal lié a plusieurs phases , ils seront tous excécuter lors de la phase.
si une phase n'a pas de goal alors elle ne s'éxecutera pas.

.. seealso:: 
    les lifecycles ont pour chacune de leurs phases des plugins avec leur goal déjà implémenté par défaut.
    
    <http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#built-in-lifecycle-bindings>

Création Projet Mvn CLI
***********************

Maven s'appuie sur des archétypes pour conserver la convention maven

 ::

    $ mvn archetype: generate -DarchetypeArtifact=<nom de l'arcchetype> -DarchetypeVersion=<numero de la version>

.. image:: _static/SqueletteMaven.png
    :width: 25%
    :align: center

.. warning:: Attention pensé a changer le JRE utilisé par maven, il ne corresponds jamais à celui d'éclipse et vérifier que l'encodage est en UTF-8

.. code-block:: xml

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>11</maven.compiler.source>
	    <maven.compiler.target>${maven.compiler.source}</maven.compiler.target>
    </properties>

Compilation & Création .Jar
***************************

Pour compiler et créer un .jar on utilise la commande suivante:
 ::

    @delaval-lenovo:~/MonApplication$ mvn package

.. image:: _static/targetMvn.png
    :width: 50%
    :align: center

Pour lancer l'application :
 ::

    @delaval-lenovo:~/MonApplication$ java -cp target/MonApplication.jar org.monpackage.laClassJava

Etude POM.xml
*************

Information Maven
=================

 .. code-block:: xml

    <!-- Information Maven -->
    <groupId>org.example.demo</groupId>
    <artifactId>MonApplication</artifactId>
    <version>1.0-SNAPSHOT</version> 
    <!--  numéro de version + Snapshot: détermine que le projet est en cours de développement -->
    <!--  une fois Snapshot enlevé : c'est une release, la version ne bouge plus! -->
    <packaging>jar</packaging>
    <!--  packaging peut etre un war pour tomcat -->
    
Information générale
====================

 .. code-block:: xml
    
    <!-- Information générale -->
    <name>MonApplication</name>
    <description>La super application qui sert à rien</description>
    <url>http://maven.apache.org</url>
    <!-- url de l'application ou la page d'accueil du projet -->

    <!-- ==========Organisation======================== -->
    <organization>
        <name>Mon Entreprise</name>
        <url>https://www.doriandelaval.fr</url>
    </organization>

    <!-- ========== Licences =========================== -->
    <licenses>
        <license>
            <name>Apache License, Version 2.0</name>
            <url>https://www.apacha.org/licences/LICENSE-2.0.txt</url>
        </license>
    </licenses>

Propriétées
===========

Ceux sont des **constantes** qui seront remplacées au moment de la compilation par mvn.

**Convention** on déclare la version de Junit en Propriétée et on la place dans les dépendances à la place du numéro de version en utilisant ${nom de la Propriétée}

 .. code-block:: xml

    <!-- ========== Propriétées ========================= -->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>${maven.compiler.source}</maven.compiler.target>
        <junit.version>3.8.1</junit.version>
    </properties>

    <dependencies>
        <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit.version}</version>
        <scope>test</scope>
        </dependency>
    </dependencies>

Propriétés pré-définies
+++++++++++++++++++++++

.. list-table:: Variables prédéfinies
   :widths: 10 80 
   :stub-columns: 1 

   * - project.basedir
     - The directory that the current project resides in.
   * - project.baseUri 
     - The directory that the current project resides in, represented as an URI. Since Maven 2.1.0
   * - maven.build.timestamp 
     - The timestamp that denotes the start of the build (UTC). Since Maven 2.1.0-M1

<https://maven.apache.org/guides/introduction/introduction-to-the-pom.html#Available_Variables>_
	
Propriétés particulières
++++++++++++++++++++++++

.. list-table:: **Variables particuliéres**
   :widths: 10 80 
   :stub-columns: 1 

   * - env
     - permet de renvoyer la valeur d'une variable d'environnement. Par exemple, **${env.PATH}**
   * - project
     - renvoie la valeur d'une balise dans le fichier pom.xml du projet. Par exemple, **${projet.organization.name}**
   * - setting
     - renvoie la valeur d'une balise dans le(s) fichier(s) settings.xml utilisé(s) par Maven.
   * - java
     - renvoie la valeur d'une propriété systeme de Java. même propriété que java.lang.System.getProperties() : exemple **${java.version}**

Le Build
========
c'est le processus de construction d'un projet par mvn, on peut configurer différement le build en modifiant certains éléments.

Par exemple, **modifier le repértoire de sortie**

 .. code-block:: xml
    
    <!--  ============================================ -->
    <!-- ===========Le Build========================== -->
    <!-- ============================================= -->
    <build>
        <directory>${project.basedir}/output</directory>
        <!--  on remplace la sortie target par un dossier ici output -->
    </build>

Par Exemple, **Mettre la class MAin dans le manifest du.jar** pour l'excécuter simplement 

 .. code-block:: xml

    <build>
        <plugins>
        <!-- ======= gestion des plugins (version)========== -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>3.2.0</version>
                <configuration>
                <archive>
                    <!-- creation du Manifest avec definition de la classe Main -->
                    <manifest>
                        <mainClass>org.example.demo.App</mainClass>
                    </manifest>
                </archive>
                </configuration>
            </plugin>
        </plugins>
    </build>

Ligne de commande pour executer directement le .jar
::

    $java -jar target/MonApplication.jar

Le Filtrage de ressources
==========================

#. créer un repertoires sous : **src/main/resources**
#. créer un fichier dedans ie: info.properties
#. Y mettre les propriétés que l'on veut récupérer avec : **${nom de la propriété}**
#. Mettre dans le build du pom.xml les informations des ressources et les filtrer pour qu'ils puissent etre utiliser ou pas : option **<filtering> à true ou false**

 
 .. code-block:: xml
    
    <build>
  	<!-- ===============resources================ -->
  	<resources>
  		<!-- repertoire permettant le filtrage des ressources -->
  		
  		<resource>
  			<directory>src/main/resources/filtered</directory>
  			<filtering>true</filtering>
  		</resource>
  		
  		<!-- repertoire n'autorisant pas le filtrage des ressources -->
  		
  		<resource>
  			<directory>src/main/resources/raw</directory>
  			<filtering>false</filtering>
  		</resource>
  	</resources>

5. Utiliser dans l'application les classes **Properties** et **InputStream** pour récupérer ses propriétés

 .. code-block:: java

    Properties versionProperties = new Properties();
	InputStream versionInputStream = null;

    try {
        versionInputStream = App.class.getResourceAsStream("/nom du fichier properties");
        versionProperties.load(versionInputStream);

    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        if (versionInputStream != null) {
            try {
                versionInputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    System.out.println("Application version =" + versionProperties.getProperty("String Key de la properties"));


Les Profiles
============

Les profiles permettent de créer des options pour le build de mvn

Example: 

#. créons deux repertoires différents un pour la prod et un pour le test **src/main/resources/conf-prod**, **src/main/resources/conf-test**
#. mettons y un fichier properties dans les deux repertoires: **même nom mais contenu différent**
#. créeons les profiles dans le pom.xml

 .. code-block:: xml

    <!--  ============================================ -->
    <!-- ===========Les Profiles====================== -->
    <!-- ============================================= -->

    <profiles>
        <profile>
            <id>prod</id>
            <build>
                <resources>
                    <resource>
                        <directory>src/main/resources/filtered/conf-prod</directory>
                        <filtering>true</filtering>
                    </resource>
                </resources>
            </build>
        </profile>
        <profile>
            <id>test</id>
            <build>
                <resources>
                    <resource>
                        <directory>src/main/resources/filtered/conf-test</directory>
                        <filtering>true</filtering>
                    </resource>
                </resources>
            </build>
        </profile>
    </profiles>

On a définit deux profiles un pour le test et un pour le prod avec un fichier properties qui aura un contenu différent en fonction du build utiliser

 ::

    $ mvn clean package -P test ou prod




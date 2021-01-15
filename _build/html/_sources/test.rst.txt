*********
Les Tests   
*********

Pyramides des tests
*******************



 .. image:: _static/pyramid-test.png
    :width: 60%
    :align: center


Les Tests d'Intégration
=======================

Les test d'intégration **vérifient que les unités de codes fonctionnent ensemble**: test unitaires doivent passés.

* execution de composants extérieures ( bdd,service web...)

Les Tests fonctionnels(End to End)
==================================

**Simulent le comportement d'un utilisateur final sur l'application, depuis l'interface utilisateur.**

Boite Noire qui ne connait pas les briques et unités de codes de l'application.

Pourquoi Tester ?
=================

* Tester pour **faire face à l'inattendu** :


    Utiliser les user stories et prévoir les scénarios alternatifs :

        **cas des limites courants et risque de non disponibilité de services extérieurs**

* Testez pour **faciliter la maintenance**:

    Eviter la **régréssion d'un code** en testant pour faciliter la maintenance et la correction de ce dernier

* Testez pour **communiquer**:

    Permet de savoir comment l'application focntionne: plus simple pour communiquer




JUnit & Test Unitaires
**********************


Les test unitaires sont crées **pour tester des fonctionnalités**, excécutés de nombreuses fois et stables.

#1 **methode Arrange_Act_Assert**

 .. image:: _static/methode_AAA.png
    :width: 60%
    :align: center

#2 **methode du Red Red Green**: la refactorisation c'est rendre le code plus lisible et/ou plus élégant sans changer son comportement pour conserver le vert ie le test validé...

 .. image:: _static/RedGreen.png
    :width: 60%
    :align: center

un exemple avec un test sur une addition: 

 .. code-block:: Java

     package calculator;
     import static org.junit.jupiter.api.Assertions.assertEquals;
     import org.junit.jupiter.api.Test;
     class CalculatorTest {
        @Test
        void testAddTwoPositiveNumbers() {
            // ARRANGE
            int a = 2;
            int b = 3;
            Calculator calculator = new Calculator();
            // ACT
            int somme = calculator.add(a, b);
            // ASSERT
            assertEquals(5, somme);
        }


JUnit & ses Annotations
***********************


@BeforeEach
===========

Exécutez une méthode avant chaque test. C’est un très bon emplacement pour installer ou organiser un prérequis pour vos tests.

@AfterEach
==========

Exécutez une méthode après chaque test. C’est un très bon emplacement pour nettoyer ou satisfaire à une postcondition.

@BeforeAll
==========

Désignez une **méthode statique** pour qu’elle soit exécutée avant tous vos tests. Vous pouvez l’utiliser pour installer d’autres variables statiques pour vos tests.

@AfterAll
=========

Désignez une **méthode statique** pour qu’elle soit exécutée après tous vos tests. Vous pouvez utiliser ceci pour nettoyer les dépendances statiques.

@ParametrizedTest
=================

Vous souhaitez réutiliser le même test avec plusieurs entrants (@ValueSource) voire plusieurs entrants/sortants (@CsvSource).

@Timeout
========

Si vous testez une méthode qui ne doit pas être trop lente, vous pouvez la forcer à échouer le test.



AssertJ
*******
c'est une librairie permettant d'utiliser des Assertions plus parlantes pour l'utilisateur.

Elle est intéressante car elle comporte différents modules à utiliser pour des cas particuliers: exemple assertJ DB Module

ci joint le lien vers la doc<https://assertj.github.io/doc/>_ 

La couverture du code
*********************
**La couverture des test = quantité de code couverte par les test / quantité de codes total**

Pour connaitre la couverture du code par les tests, il faut d'abord savoir sur quel critére on se base pour compter la quantité de code :

 * le nombre de lignes
 * le nombres d'instructions
 * le nombre de branches (ensemble d'instruction IF/else Try/catch
 * le nombre de methodes/fonctionnels

EclEmma(Eclipse)
================

il suffit d'utiliser le "coverage as " JUnit Test sur la classe Test.

 .. image:: _static/eclEmma.png
    :width: 60%

résultat s'affiche avec le pourcentage total aussi bien avec src/main que src/test qui nous intéresse pas puisque c'est le code du test

.. image:: _static/CoverageEclEmma.png

JACOCO
======

**Java Codes Coverage**

Donne un rapport sur page html avec uniquement le pourcentage de couverture de code sur le src/main

Config
++++++

rajouter le plugin **jacoco-maven-plugin**
 .. code-block:: xml

    <plugin>
		<groupId>org.jacoco</groupId>
		<artifactId>jacoco-maven-plugin</artifactId>
		<version>0.8.5</version>
		<executions>
			<execution>
				<goals>
					<goal>prepare-agent</goal>
				</goals>
			</execution>
			<execution>
				<id>report</id>
				<phase>test</phase>
				<goals>
					<goal>report</goal>
				</goals>
			</execution>
		</executions>
	</plugin>

Excecution
++++++++++
 Executer dans le terminal:
 ::
   
    $ mvn clean package


ce qui va créer dans **target/ un fichier index.html** avec le rapport de jacoco
*******
Mockito
*******
Un mock sert à simuler une classe A pour pouvoir tester un autre classe B qui utilise la classe A.
démarche a suivre pour déterminer les mocks:

* Identifiez un comportement unique que vous testez avec votre classe sous-test (CUT).
* Demandez-vous quelles classes sont nécessaires au comportement à tester.
* Hormis votre CUT, envisagez toutes les autres classes pour le mocking.
* Ne mockez pas les classes qui ne servent quasiment qu’à porter des valeurs.
* Installez les mocks requis.
* Testez votre CUT.
* Vérifiez que vos mocks ont été correctement utilisés.

Config Mvn
**********

on modifie le **pom.xml**

 .. code-block:: xml

    <dependency>
		<groupId>org.junit.jupiter</groupId>
		<artifactId>junit-jupiter-api</artifactId>
		<version>5.5.1</version>
		<scope>test</scope>
	</dependency>  
    ...
    <dependency>
	    <groupId>org.mockito</groupId>
	    <artifactId>mockito-junit-jupiter</artifactId>
	    <version>3.7.7</version>
	    <scope>test</scope>
	</dependency>
 
Config eclipse
**************
Pour l'autocompletion des elelements de mockito:

Dans **windows/preferences/java/editor/ContentAssist/Favorites** ajouter:

* org.Mockito.ArgumentMatchers.*
* org.mockito.Mockito.*

Creation d'un Mock
******************
 
Déclaration
===========

Utilisation dans la classe test d'un mock de l'annotation de Junit **@ExtendWith(MockitoExtension.class) et @Mock**

 .. code-block:: java

    @ExtendWith(MockitoExtension.class)

    public class CalculatorServiceTest {

      @Mock
      Calculator calculator;

Utilisation
============

Static
++++++

Un exemple d'utilisation dans une methode de test:

* **GIVEN** : on utilise le mock pour **simuler avec "When the x method is called then return y"**
* **WHEN** : on récupére le résultat de la methode de la classe a tester
* **THEN** : 
            * on verifie que **la classe mockée a bien été utilisée avec verify(mock).methode();** 
            * on verifie avec **asserThat** que le resultat de la methode de la classe a tester est correct

 .. code-block:: java
     
    @Test
  public void calulate_shouldUseCalculator_forMultiply() {
    // GIVEN
    when(calculator.multiply(1, 2)).thenReturn(2);

    // WHEN
    int result = classUnderTest
        .calculate(new CalculationModel(CalculationType.MULTIPLICATION, 1, 2)).getSolution();

    // THEN
    verify(calculator).multiply(1, 2);
    assertThat(result).isEqualTo(2);
  }

Générique
+++++++++
any()
~~~~~

on peut simuler le mock sans paramètres précis avec **any()** 

 .. code-block:: java
    
    when(calculator.add(any(Integer.class), any(Integer.class))).thenReturn(3);

dans tous les cas , cela donnera 3 comme résultat et on peut faire de même avec la methode **verify()**

.. code-block:: java

    verify(calculator).add(any(Integer.class), any(Integer.class));

times()/never()
~~~~~~~~~~~~~~~

on peut verifier combien de fois la methode du mock a été utilisée avec times() en paramètres

.. code-block:: java

    verify(calculator,times(1)).add(any(Integer.class), any(Integer.class));

ici il faut qu'elle soit appelée 1 fois seulement si on ne veut pas qu'elle soit appelé on utilise soit:
* times(0)
* never()

Exceptions
++++++++++
Si on veut configurer un mock pour lancer une exception on remplace **thenReturn() par thenThrows()**.

Et pour le When on utilisera **assertThrows()** pour verifier si une exception est levée par la methode de la CUT: 

 .. code-block:: java

    @Test
    public void calculate_shouldThrowIllegalArgumentException_forADivisionBy0() {
	// GIVEN
	when(calculator.divide(1, 0)).thenThrow(new ArithmeticException());

	// WHEN
	assertThrows(IllegalArgumentException.class, () -> classUnderTest.calculate(
			new CalculationModel(CalculationType.DIVISION, 1, 0)));

	// THEN
	verify(calculator, times(1)).divide(1, 0);
    }


Fonctions avancées
******************

ArgumentCaptor
==============




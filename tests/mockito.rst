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

1) Static
+++++++++

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

2) Générique
++++++++++++ 
a) any()
~~~~~~~~

on peut simuler le mock sans paramètres précis avec **any()** 

 .. code-block:: java
    
    when(calculator.add(any(Integer.class), any(Integer.class))).thenReturn(3);

dans tous les cas , cela donnera 3 comme résultat et on peut faire de même avec la methode **verify()**

 .. code-block:: java

    verify(calculator).add(any(Integer.class), any(Integer.class));

b) times()/never()
~~~~~~~~~~~~~~~~~~

on peut verifier combien de fois la methode du mock a été utilisée avec times() en paramètres

 .. code-block:: java

    verify(calculator,times(1)).add(any(Integer.class), any(Integer.class));

ici il faut qu'elle soit appelée 1 fois seulement si on ne veut pas qu'elle soit appelé on utilise soit:
* times(0)
* never()

3) Exceptions
+++++++++++++
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

Permet **d'enregistrer les arguments utilisés lors de l'appel du mock** pour par la suite en assertion verifier par exemple que ce sont les bons arguments escomptés

1) Déclaration
++++++++++++++
Exemple sur la classe CalculationModel:

 .. code-block:: java

    final ArgumentCaptor<CalculationModel> calculationModelCaptor =
    ArgumentCaptor.forClass(CalculationModel.class);

2) Verification
+++++++++++++++
On verifie que calculatorService a bien utilisé les arguments de calculationModel
et ensuite on récupere dans une liste cette suite d'argument.

 .. code-block:: java

    verify(calculatorService, times(4)).calculate(calculationModelCaptor.capture());
    final List<CalculationModel> calculationModels = calculationModelCaptor.getAllValues();

3) Cas de Verification
++++++++++++++++++++++

Le but est juste de vérifier si l'appel de la methode a tester a bien récupérer le bon nombre d'argument et si ils ont correctement été récupérés:

* **//GIVEN** :on capture les arguments CaculculationModel utilisé par batchCalculatorService lors de l'appel de la methode batchCalculate() avec calculationModelCaptor

* **//THEN** :on verifie que lorsqu'on appelle batchCalculate (operations) , il y a bien eu 4 appel de calculatorService et on capture les calculationModels avec calculationModelCaptor.capture()

* **//THEN** : on verifie que les calculationModels capturés (recupérés avec calculatioModelCaptor.getAllValue() correspondent au tuple que l'on a ecrit en dure par rapport à la liste d'opérations qu'on a dans // GIVEN. cela se fait en utilisant extracting ( <class> :: <methode get>) voir les lambdas...


 .. code-block:: java

    @Test
    public void givenOperationsList_whenbatchCalculate_thenCallsServiceWithCorrectArguments()
    throws IOException, URISyntaxException {
      // GIVEN
      final Stream<String> operations =
          Arrays.asList("2 + 2", "5 - 4", "6 x 8", "9 / 3").stream();
      final ArgumentCaptor<CalculationModel> calculationModelCaptor =
          ArgumentCaptor.forClass(CalculationModel.class);

      // WHEN
      batchCalculatorService.batchCalculate(operations);

      // THEN
      verify(calculatorService, times(4)).calculate(calculationModelCaptor.capture());
      final List<CalculationModel> calculationModels = calculationModelCaptor.getAllValues();
      assertThat(calculationModels)
          .extracting(CalculationModel::getLeftArgument, 
                      CalculationModel::getType,
                      CalculationModel::getRightArgument)
          .containsExactly(
              tuple(2, CalculationType.ADDITION, 2),
              tuple(5, CalculationType.SUBTRACTION, 4),
              tuple(6, CalculationType.MULTIPLICATION, 8),
              tuple(9, CalculationType.DIVISION, 3));
    }

InvocationOnMock
================

on veut **donner une réponse spécifique en fonction de n'importe quel argument entré** lors de l'appel du mock pour ensuite verifier le résultat de notre methode du CUT.

on utilise **l'interface invocation heritant de InvocationOnMock que l'on redéfinit avec une lambdas** pour déclarer le résultat que l'on souhaite en fonction des arguments données a notre mock.

Deux alternatives s'offre a nous :

1) lambdas avec Invocation
++++++++++++++++++++++++++

 .. code-block:: java

    @Test
    public void givenOperationsList_whenbatchCalculate_thenCallsServiceAndReturnsAnswer()
        throws IOException, URISyntaxException {
      // GIVEN
      final Stream<String> operations = Arrays.asList("2 + 2", "5 - 4", "6 x 8", "9 / 3").stream();
      when(calculatorService.calculate(any(CalculationModel.class)))
      .then(invocation -> {
            final CalculationModel model = invocation.getArgument(0, CalculationModel.class);
            switch (model.getType()) {
            case ADDITION:
              model.setSolution(4);
              break;
            case SUBTRACTION:
              model.setSolution(1);
              break;
            case MULTIPLICATION:
              model.setSolution(48);
              break;
            case DIVISION:
              model.setSolution(3);
              break;
            default:
            }
            return model;
          });

      // WHEN
      final List<CalculationModel> results = batchCalculatorService.batchCalculate(operations);

      // THEN
      verify(calculatorService, times(4)).calculate(any(CalculationModel.class));
      assertThat(results).extracting("solution").containsExactly(4, 1, 48, 3);

    }

2) Les Then() chainés
+++++++++++++++++++++

on vas simplement mettre a la suite des then() qui corrrespondront respectivement a leur arguments entrées

 .. code-block:: java
 
    @Test
	  public void givenOperationsList_whenbatchCalculate_thenCallsServiceAndReturnsAnswer2()
			throws IOException, URISyntaxException {
		// GIVEN
		final Stream<String> operations =
		    Arrays.asList("2 + 2", "5 - 4", "6 x 8", "9 / 3").stream();
		
    when(calculatorService.calculate(any(CalculationModel.class)))
				.thenReturn(new CalculationModel(CalculationType.ADDITION, 2, 2, 4))
				.thenReturn(new CalculationModel(CalculationType.SUBTRACTION, 5, 4, 1))
				.thenReturn(new CalculationModel(CalculationType.MULTIPLICATION, 6, 8, 48))
				.thenReturn(new CalculationModel(CalculationType.DIVISION, 9, 3, 3));

		// WHEN
		final List<CalculationModel> results =
		    batchCalculatorService.batchCalculate(operations);

		// THEN
		verify(calculatorService, times(4)).calculate(any(CalculationModel.class));
		assertThat(results).extracting("solution").containsExactly(4, 1, 48, 3);

	}


@Spy
====

On ne peut pas mocker une methode d'une classe définit en final avec @Mock 
mais on peut tout de meme utiliser @spy sur la classe pour récupérer des méthodes non final.

En gros , c'est un mock partiel de la classe
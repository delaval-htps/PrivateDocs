*******************
Tests d'Intégration
*******************

tests qui permettent de verifier la cohérence et efficacité de l'ensemble des composants du code (classes)
Il en existe 2 types

Les test d'intégration Composants
*********************************
permettent de verifier si plusieurs unités de code fonctionnent entre elles dans un environnement isolé sans lien avec des composants extérieurs

Création d'un IT composant
==========================

1. Convention
+++++++++++++
Par convention on nomme une classe de test d'intégration **avec IT à la fin**

exemple: **CalculatorServiceIT.java**

2. Création
+++++++++++

C'est le même principe qu'un **test unitaire mais on n'utilise pas de mock**.

On crée directement une instance de la classe  dont on a besoin pour notre CUT dans la partie //GIVEN:

 .. code-block:: java

    package com.openclassrooms.testing.calcul.service;

    import static org.assertj.core.api.Assertions.assertThat;

    public class CalculatorServiceIT {

	// Mettre en place des objets réels non mockés
	private final Calculator calculator = new Calculator();
	private final SolutionFormatter formatter = new SolutionFormatterImpl();

	// Initialiser la classe à tester
	private final CalculatorService underTest = new CalculatorServiceImpl(calculator, formatter);

	@Test
	public void calculatorService_shouldCalculateASolution_whenGivenACalculationModel() {
		// GIVEN
		final CalculationModel calculation = new CalculationModel(CalculationType.ADDITION,
				100, 101);
		// WHEN
		final CalculationModel result = underTest.calculate(calculation);

		// THEN
		assertThat(result.getSolution()).isEqualTo(201);
	    }
    }

3. lancement des IT
+++++++++++++++++++

On utilise Maven , sachant que **mvn package**, nous fait un build , les tests unitaires et le packaging des sources.

Ici on va utiliser:

::
    
    $mvn verify

Les Test d'intégration Système (SIT)
************************************
permettent de vérifier le fonctionnement de plusieurs unités au sein d'une config d'application avec des liens à des composants exterieures tels que BDD fichiers, API en réseau...

Convention
==========

**On utilisera le FrameWork Spring qui propose des outils pour les SIT.**
le nom de classe se termine par **SIT** pour avoir le IT a la fin et que mvn puisse faire la différence avec les test unitaires

Annotations Spring
==================
Du coup il faut utiliser les annotations suivantes:

1. @WebMvcTest
++++++++++++++

Pour Initialiser les classes utilisées sur lesquels ont veux faire le test SIT ( verifier le comportement ) 

2. @ExtendWith
++++++++++++++
on utilise (SpringExtension.class) : c'est Spring qui gére directemetnles Mocks et non mockito

3. @Inject
++++++++++
On va injecter le service MockMvc (@Inject = @Autowired mais plus générique)
l'injection de dépendance peut se faire parceque nous avons déclarer les classes injectées avec:
	* @service
	* @Component
	* @controller
	* @Bean 
	* @Named, la plus générique de toute

4.@MockBean
+++++++++++
on demande à Spring de nous créer les Mocks nécessaire a la réalisation du test

Exemple de test SIT
===================

 .. code-block:: java

	@WebMvcTest(controllers = {CalculatorController.class, CalculatorService.class})
	@ExtendWith(SpringExtension.class)
	public class CalculateurControllerSIT {

	@Inject
	private MockMvc mockMvc;

	@MockBean
	private SolutionFormatter solutionFormatter;

	@MockBean
	private Calculator calculator;

	@Test
	public void givenACalculatorApp_whenRequestToAdd_thenSolutionisShow() throws Exception {
		// GIVEN on mock calculatr et solutionFormatter
		when(calculator.add(3, 2)).thenReturn(5);
		when(solutionFormatter.format(5)).thenReturn("5");


    // WHEN on récupére le résultat de la simulation de la requete http et on vérifie le succes
    MvcResult result = mockMvc
        .perform(MockMvcRequestBuilders.post("/calculator").param("leftArgument", "3")
            .param("calculationType", "ADDITION").param("rightArgument", "2"))
        .andExpect(MockMvcResultMatchers.status().is2xxSuccessful()).andReturn();

    // THEN on verifie les utilisations des mocks et que le résultat est bien 5 dans la réponse html avec l'id solution
    verify(calculator, times(1)).add(3, 2);
    verify(solutionFormatter, times(1)).format(5);
    assertThat(result.getResponse().getContentAsString()).contains("id=\"solution\"")
        .contains(">5</span");

  		}
	}




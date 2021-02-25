******************
Tests fonctionnels
******************

Appelés aussi **End to End**, ils servent a obtenir le bon résultat en partant de l'interface utilisateur, selon un scénario prédéfini.

Ces tests utilisent une automatisation soit:

    * par navigateur avec **WebDriver de Selenium** <https://github.com/bonigarcia/webdrivermanager/>
    * par interface mobile avec **Appium**.<http://appium.io/>

Configuration
*************

nomenclature des fichiers
=========================
Créer un package pour le test fonctionnel:
    * **package com.openclassrooms.testing.calcul.e2e**
    * une classe test **<nomDuTestE2E>** pour End to End. 

Annotations Spring
==================

Forcément, ils sont implémentés dans une application Spring. il faut donc utiliser les annotations suivantes:

    * **@ExtendWith(SpringExtension.class)** : extension de Junit 5 pour Spring
    * **@SpringBootTest** pour démarrer l'application avec SpringBootTest

Parametrage de WebDriver
========================

On utilise webdrivermanager pour initialiser notre navigateur de test (port,setup, navigateur,url)

    * **@SpringBootTest(webEnvironnement = SpringBootTest.WebEnvironnement.RANDOM_PORT)** pour indiquer d'utiliser un port aléatoire autre que ce utiliser par le seveur ou autre application

    * Pour déclarer un port

        .. code-block:: java

            @LocalServerPort
            private Integer port;

    * Pour initialiser le driver pour le navigateur firefox

        .. code-block:: java

            @BeforeAll
            public static void setUpFirefoxDriver() {
                WebDriverManager.firefoxdriver().setup();
            }

    * Pour créer une instance du driver pour le navigateur firefox

        .. code-block:: java

            @BeforeEach
            public void setUpWebDriver() {
                webDriver = new FirefoxDriver();
                baseUrl = "http://localhost:" + port + "/calculator";
            }

    * Enfin pour quitter proprement le driver pour le navigateur firefox
      
        .. code-block:: java
            
            @AfterEach
            public void quitWebDriver() {
                if (webDriver != null) {
                    webDriver.quit();
                }
            }

Example de test E2E
*******************

Ce qui nous donne une classe comme celle ci

.. code-block:: java

    package com.openclassrooms.testing.calcul.e2e;

    import ...

    @ExtendWith(SpringExtension.class)
    @SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
    public class StudentMultiplicationJourneyE2E {

	@LocalServerPort
	private Integer port;
	private WebDriver webDriver = null;
	private String baseUrl;

	@BeforeAll
	public static void setUpFirefoxDriver() {
		WebDriverManager.firefoxdriver().setup();
	}

	@BeforeEach
	public void setUpWebDriver() {
		webDriver = new FirefoxDriver();
		baseUrl = "http://localhost:" + port + "/calculator";
	}

	@AfterEach
	public void quitWebDriver() {
		if (webDriver != null) {
			webDriver.quit();
		}
	}

	@Test
	public void aStudentUsesTheCalculatorToMultiplyTwoBySixteen() {

		// GIVEN
		webDriver.get(baseUrl);
		final WebElement leftField = webDriver.findElement(By.id("left"));
		final WebElement typeDropdown = webDriver.findElement(By.id("type"));
		final WebElement rightField = webDriver.findElement(By.id("right"));
		final WebElement submitButton = webDriver.findElement(By.id("submit"));

		// WHEN
		leftField.sendKeys("2");
		typeDropdown.sendKeys("x");
		rightField.sendKeys("16");
		submitButton.click();

		// THEN
		final WebDriverWait waiter = new WebDriverWait(webDriver, 5);
		final WebElement solutionElement = waiter.until(
				ExpectedConditions.presenceOfElementLocated(By.id("solution")));
		final String solution = solutionElement.getText();
		assertThat(solution).isEqualTo("32"); // 2 x 16
	    }
    }

PageObject
**********

Pour eviter de refactoriser des test E2E qui seraient similaire et pour eviter que tout pete si on modifie la page du navigateur, on utilise une pageObject.

Le principe est de créer une page web et de récupérer depuis cette dernière les elements qui nous intéresse

On crée une pageObject avec une classe <CalulatorPage> dans le package E2E

Déclaration des attributs
=========================
On récupére les id qui nous intéresse:

exemple:

.. code-block::java

    public class CalculatorPage {

	@FindBy(id = "submit")
	private WebElement submitButton;

	@FindBy(id = "left")
	private WebElement leftArgument;

	@FindBy(id = "right")
	private WebElement rightArgument;

	@FindBy(id = "type")
	private WebElement calculationType;

	@FindBy(id = "solution")
	private WebElement solution;
	
	...
    }

Methode d'operation PageObject
==============================

une fois les elements récupérés , il nous faut appeler le resultat en ajoutant depuis notre PageObject une methode permettant de lancer le travail quel doit faire avec les elements. exemple pour le calcul:

.. code-block:: java

    private String calculate(String calculationTypeValue, String leftValue, String rightValue) {
    leftArgument.sendKeys(leftValue);
    calculationType.sendKeys(calculationTypeValue);
    rightArgument.sendKeys(rightValue);
    submitButton.click();

    final WebDriverWait waiter = new WebDriverWait(webDriver, 5);
    waiter.until(ExpectedConditions.visibilityOf(solution));

    return solution.getText();
	}


on peut ensuite créer des methodes récupérant les id qui nous intéresse et appeler la méthode d'operation ici calcul avec ces arguments

.. code-block:: java

    public String add(String leftValue, String rightValue) {
		return calculate(ADDITION_SYMBOL, leftValue, rightValue);
	}

cette methode récupére les id de la page web leftvalue et rightvalue pour faire l'addition en appelant notre methode opération

Modification classE2E
=====================

On peut maintenant simplifier notre class test E2E en remplacant les GIVEN WHEN et THEN par l'appel de notre PageObject

Avant:

.. code-block:: java

    @Test
	public void aStudentUsesTheCalculatorToAddTwoBySixteen() {

		// GIVEN
		webDriver.get(baseUrl);
		final WebElement leftField = webDriver.findElement(By.id("left"));
		final WebElement typeDropdown = webDriver.findElement(By.id("type"));
		final WebElement rightField = webDriver.findElement(By.id("right"));
		final WebElement submitButton = webDriver.findElement(By.id("submit"));

		// WHEN
		leftField.sendKeys("2");
		typeDropdown.sendKeys("+");
		rightField.sendKeys("16");
		submitButton.click();

		// THEN
		final WebDriverWait waiter = new WebDriverWait(webDriver, 5);
		final WebElement solutionElement = waiter.until(
		ExpectedConditions.presenceOfElementLocated(By.id("solution")));
		final String solution = solutionElement.getText();
		assertThat(solution).isEqualTo("32"); // 2 + 16
	}

Apres:

.. code-block:: java

    @Test
	public void aStudentUsesTheCalculatorToAddTwoToSixteen() throws InterruptedException {
		// GIVEN
		webDriver.get(baseUrl);
		final CalculatorPage calculatorPage = new CalculatorPage(webDriver);

		// WHEN
		final String solution = calculatorPage.add("2", "16");

		// THEN
		assertThat(solution).isEqualTo("18"); // 2 + 16
	}




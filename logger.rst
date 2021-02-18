******
Log4j2
******

Config Maven
************
Ajouter les dépendences pour log4j2 dans le pom.xml

 .. code-block:: xml

    <dependencies>

		<!-- ===================== Log4j2 ====================== -->
		  <dependency>
		    <groupId>org.apache.logging.log4j</groupId>
		    <artifactId>log4j-api</artifactId>
		    <version>2.14.0</version>
		  </dependency>
		  <dependency>
		    <groupId>org.apache.logging.log4j</groupId>
		    <artifactId>log4j-core</artifactId>
		    <version>2.14.0</version>
		  </dependency>

Config Log4j2
*************
On crée un fichier **log4j2.xml** de configuration que l'on place dans src/main/java/resources

 .. warning: sous Eclipse , il sera dans src/resources car eclipse considére que la source = src/main/java

l'example ci dessous, montre une sotie dirigée vers la console avec un script qui permet de modifier le pattern en fonction des levels utilisés.

il emploie également le **highlight** pour coloriser les lignes.

Attention: utillisation d'une console ansi sinon il ne le detécte pas !!! 

 .. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE xml>
    <Configuration>
    <Appenders>
        <Console name="STDOUT" target="SYSTEM_OUT">
        <PatternLayout>
        <ScriptPatternSelector defaultPattern="%highlight{%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n}{ERROR=red,TRACE=blue}">
                        <Script name="LevelSelector" language="javascript"><![CDATA[
                            result= null;
                            if (logEvent.getLevel() == org.apache.logging.log4j.Level.INFO) {
                                result="INFO";
                            }
                        
                            ]]>
                        </Script>
                        <PatternMatch key="INFO" pattern="%msg%n"/>
                        
                    </ScriptPatternSelector>
        </PatternLayout>
    <!--       <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/> -->
        </Console>
    </Appenders>
    <Loggers >
        <Root level="DEBUG">
        <AppenderRef ref="STDOUT"/>
        </Root>
    </Loggers>
    </Configuration>



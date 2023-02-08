******
Thread
******

Définition
**********

Les threads sont des objets qui sont exécutés simultanément dans une seule JVM partageant sa mémoire et d'autres ressources. Ils sont exécutés **de manière asynchrone**, hors de votre contrôle, et dépendent  même du planificateur de threads  du JRE pour s'exécuter.  

Etats
*****
Les threads peuvent avoir l'un des nombreux  **ThreadStates**  en fonction de ce que le planificateur de threads a décidé d'en faire :

+-----------+-----------------------------------------------------------------------------------+
| Etats     | définition                                                                        |
+===========+===================================================================================+
|NEW        |  le Thread  a été créé mais n'a pas été soumis au planificateur.                  |
+-----------+-----------------------------------------------------------------------------------+
|RUNNABLE   | le thread est actuellement exécuté par la JVM.                                    |
+-----------+-----------------------------------------------------------------------------------+
|TIME WAIT  |  le thread dort pendant un intervalle fixe.                                       |
+-----------+-----------------------------------------------------------------------------------+
|WAITING    |   le thread a été suspendu pendant qu'il attend une condition pour le réactiver.  |
+-----------+-----------------------------------------------------------------------------------+
|BLOCKED    |  le thread est en train de passer de WAITING à RUNNABLE.                          |
+-----------+-----------------------------------------------------------------------------------+
|TERMINATED |le thread a terminé son travail et a été libéré par le planificateur.              |
+-----------+-----------------------------------------------------------------------------------+

Runnable
********
Pour créer un thread , on a trois solutions:

1. Utilisation d'un **lambda** .

    .. code-block:: java
        
        Thread t = new Thread(()->System.out.println("Hello Openclassrooms Student!"));

2. Implémenter l'interface **Runnable** , c'est-à-dire définir une méthode run().

    .. code-block:: java
        
        class RandomRunnable implements Runnable {
            @Override
            public void run() {
                System.out.println("Hello Openclassrooms Student!");
            }
        }

        Thread t = new Thread(new RandomRunnable());
        t.start();

3. Extend directement un Thread puisqu'il s'agit d'un Runnable.

    .. code-block:: java

        class RandomThread extends Thread {
            @Override
            public void run() {
                System.out.println("Hello Openclassrooms Student!");
            }
        }
        RandomThread t = new RandomThread();
        t.start();

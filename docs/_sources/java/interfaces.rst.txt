**********
interfaces
**********

Definition
**********
Une interface décrit un ensemble de methodes en fournissant uniquement leur signature (methode abstraite).

C'est une classe qui permet donc de définir un "comportement à suivre" ou autorise une ensemble d'interactions autorisées pour une classe qui l'implémente.

C'est dernière implementant une interface se doit d'implementer les mehtodes "abstraites" de la dite interface.

Le mechanisme des interfaces permet aussi de'introduire une forme simplifiée d'heritage multiple


interface fonctionnelle
***********************

c'est une interface qui comporte une **unique** methode abstraite. On peut donc utiliser une lambdas expression lors de son appel.

example de code de gestion d'evenement en javaFx:

Lorqu'on utilise la methode **setOnAction()** pour un button,cette dernière prend en parametre une interface fonctionelle suivante:

.. code-block:: java

    package javafx.event;

    import java.util.EventListener;

    @FunctionalInterface
    public interface EventHandler<T extends Event> extends EventListener {
        void handle(T var1);
    }

On doit donc utiliser une classe anonyme pour pouvoir l'utiliser comme ci dessous:

.. code-block:: java
        
        myButton.setOnAction(new EventHandler<ActionEvent>() {
            @Override
            public void handle(ActionEvent e) {
                totalGirlFriendCount++;
                statusLabel.setText("Nombre de copines:"+totalGirlFriendCount);
            }
        });

Au lieux d'utiliser une classe anonyme on va donc utiliser une lambdas expression

.. code-block:: java

     addGirlButton.setOnAction((e)->{
            totalGirlFriendCount++;
            statusLabel.setText("Nombre de copines:"+totalGirlFriendCount);
        });

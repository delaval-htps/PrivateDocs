***********
Collections
***********

Généralités
***********

généricité
++++++++++

Par definition, les collections utilisent la **généricité** pour contenir des Objets.

.. code-block:: java
    
    Collection<E> collection ;

Donc, dans une collection, on ne peut utiliser que le même type d'Objet.

références d'Objet
++++++++++++++++++

Lorsqu'on introduit un Objet dans une collection, on ne fait pas de copie !

On utilise la référence de l'Objet. Ce qui est logique, puisque cela permet de modifier les contenus directement.


Methodes communes
*****************


HashMap
*******

itération
+++++++++

If you're only interested in the keys, you can iterate through the keySet() of the map:

.. code-block:: java
    
    Map<String, Object> map = ...;

    for (String key : map.keySet()) {
    // ...
    }

If you only need the values, use values():

.. code-block:: java

    for (Object value : map.values()) {
    // ...
    }

Finally, if you want both the key and value, use entrySet():

.. code-block:: java

    for (Map.Entry<String, Object> entry : map.entrySet()) {
        String key = entry.getKey();
        Object value = entry.getValue();
        // ...
    }

Following are 4 common implementation of Map interface in Java,

HashMap: No ordering & No preservation of insertion order on keys/values

LinkedHashMap: Preserves the insertion order

TreeMap: Ordered by the key

HashTable: Unlike HashMap, it is synchronized

Refer here for better understanding with examples.
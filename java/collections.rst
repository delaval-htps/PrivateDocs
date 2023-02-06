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

Comparaison de deux elements
++++++++++++++++++++++++++++

compareTo()
===========

Si l'on veut comparer des objets de type String,File ou Wrapper d'une collection:

On peut utiliser la methode **compareTo(E object)** puisqu'ils implementent l'interface **Comparable**

.. warning:: Cependant, si on crée nos propres Object alors il faut leur faire implementer cette interface pour pouvoir les comparer avec cette methode.

.. code-block:: java

    public class CompareElements {

  public static void main(String[] args) {

    List<Integer> list = Arrays.asList(1, 2, 3, 3, 2, 1);

    for (int i = 0; i < list.size() - 1; i++) {
      System.out.println(
          "comparaison "
              + list.get(i)
              + " et "
              + list.get(i + 1)
              + ": "
              + list.get(i).compareTo(list.get(i + 1)));
    }
  }

.. code-block:: sh
    
    comparaison 1 et 2: -1
    comparaison 2 et 3: -1
    comparaison 3 et 3: 0
    comparaison 3 et 2: 1
    comparaison 2 et 1: 1

Comparator
==========
On peut créer un Objet de type Interface **Comparator** qui n'a qu'une seule methode **compare(E o1, E o2)** qu'il faudra redefinir.

pour comparer chaque valeur de la collection. Ce type d'Objet s'appelle un **un Objet Fonction**

.. code-block:: java

    public class ComparatorObject {

        public static void main(String[] args) {

            List<Integer> list = Arrays.asList(1, 2, 3, 3, 2, 1);

            Comparator<Integer> comparator =
                new Comparator<Integer>() {

                @Override
                public int compare(Integer arg0, Integer arg1) {
                    if (arg0.equals(arg1)) {
                    return 0;
                    } else if (arg0 > arg1) {
                    return 1;
                    } else {
                    return -1;
                    }
                }
                };
            for (int i = 0; i < list.size() - 1; i++) {
            System.out.println(
                "comparaison "
                    + list.get(i)
                    + " et "
                    + list.get(i + 1)
                    + ": "
                    + comparator.compare(list.get(i), list.get(i + 1)));
            }
        }
    }


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
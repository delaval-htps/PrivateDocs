*******
Java IO
*******

Fermeture
*********

finally
=======

The variable fr only has scope within the try block. It is out of scope in the finally block. You need to declare it before the try block:

.. code-block:: Java

    FileReader fr = null;
    try {
        fr = new FileReader(file);
        BufferedReader br = new BufferedReader(fr);
        String line = null;
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } finally {
        if (fr != null) {
            try {
                fr.close();
            } catch (IOException e) {
                // This is unrecoverable. Just report it and move on
                e.printStackTrace();
            }
        }
    }
    
This is quite a common pattern of code, so it's good to remember it for future similar situations.

Consider throwing IOException from this method - printing track traces isn't very helpful to callers, and you wouldn't need the nested try catch around fr.close()
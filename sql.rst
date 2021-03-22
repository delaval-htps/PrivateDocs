************
Relation SQL
************

One to One
**********

La relation 1:1 est semblable a celle de 1:Many , elle peut s'effectuer de deux maniéres:

* Créer un FK avec le PK de chaque table dans les 2 tables: cela consiste a créer deux 1:M entre les deux tables

* créer un Fk unique dans une des tables mais avec son id (pointant sur l'id de la seconde table

example: 
.. code-block:sql

    DROP DATABASE IF EXISTS test;
    CREATE DATABASE test CHARSET = utf8 COLLATE = utf8_general_ci;
    USE test;

    CREATE TABLE users(
    id INT NOT NULL AUTO_INCREMENT,
    user_name VARCHAR(45) NOT NULL,
    PRIMARY KEY(id)
    ) ENGINE = InnoDB DEFAULT CHARSET = utf8;

    CREATE TABLE accounts(
    id INT NOT NULL AUTO_INCREMENT,
    account_name VARCHAR(45) NOT NULL,
    PRIMARY KEY(id),
    FOREIGN KEY(id) REFERENCES users(id)
    ) ENGINE = InnoDB DEFAULT CHARSET = utf8;
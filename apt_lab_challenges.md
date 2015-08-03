Show all the psql users. (Hint: Look for a command to show roles)

apartmentlab=# \dg
                               List of roles
   Role name   |                   Attributes                   | Member of 
---------------+------------------------------------------------+-----------
 taranamolkaur | Superuser, Create role, Create DB, Replication | {}

Show all the tables in your apartmentlab database.

apartmentlab=# \dt
              List of relations
 Schema |    Name    | Type  |     Owner     
--------+------------+-------+---------------
 public | owners     | table | taranamolkaur
 public | properties | table | taranamolkaur
(2 rows)

Show all the data in the owners table.

apartmentlab=# SELECT * FROM owners
apartmentlab-# ;
 id | name | age 
----+------+-----


Add three owners: Donald (age 56), Elaine (age 24), and Emma (age 36).

apartmentlab=# INSERT INTO owners (name, age)
apartmentlab-# VALUES ('Donald', 56), ('Elaine', 24), ('Emma', 36);

Show the names of all owners.

apartmentlab=# SELECT name FROM owners; 
  name  
--------
 Donald
 Elaine
 Emma

Show the ages of all of the owners in ascending order.

apartmentlab=# SELECT name, age FROM owners ORDER BY age;
  name  | age 
--------+-----
 Elaine |  24
 Emma   |  36
 Donald |  56

Show the name of an owner whose name is Donald.

apartmentlab=# SELECT name FROM owners WHERE name = 'Donald';
  name  
--------
 Donald

Show the age of all owners who are older than 30.

apartmentlab=# SELECT name, age  FROM owners WHERE age > 30;
  name  | age 
--------+-----
 Donald |  56
 Emma   |  36

Show the name of all owners whose name starts with an "E".

apartmentlab=# SELECT * FROM owners WHERE name LIKE 'E%';
 id |  name  | age 
----+--------+-----
  2 | Elaine |  24
  3 | Emma   |  36

Add an owner named John who is 33 years old.
Add an owner named Jane who is 43 years old.

apartmentlab=# INSERT INTO owners
(name, age) 
VALUES ('John', 33), ('Jane', 43);
INSERT 0 2

Change Jane's age to 30.

apartmentlab=# UPDATE owners 
apartmentlab-# SET age = 30
apartmentlab-# WHERE name = 'Jane';

Change Jane's name to Janet.

apartmentlab=# UPDATE owners 
apartmentlab-# SET age = 30
apartmentlab-# WHERE name = 'Jane';

Delete the owner named Janet.

apartmentlab=# DELETE FROM owners 
apartmentlab-# WHERE name  = 'Janet';

Add a property named Archstone that has 20 units.
Add two more properties with names and number of units of your choice.

apartmentlab=# INSERT INTO properties (name, num_units)
VALUES ('Archstone', 20), ('Tara', 50), ('Michelle', 30);

Show all of the properties in alphabetical order that are not named Archstone.

apartmentlab=# SELECT * FROM properties              
WHERE name <> 'Archstone' ORDER BY name;
 id |   name   | num_units | owner_id 
----+----------+-----------+----------
  3 | Michelle |        30 |         
  2 | Tara     |        50 |         

Count the total number of rows in the properties table.

apartmentlab=# SELECT COUNT (*) FROM properties;
 count 
-------
     3

Show the highest age of all the owners.

apartmentlab=# SELECT MAX(age) FROM owners;
 max 
-----
  56

Show the names of the first three owners in your owners table.

apartmentlab=# SELECT name FROM owners LIMIT 3
apartmentlab-# ;
  name  
--------
 Donald
 Elaine
 Emma

Use a FULL OUTER JOIN to show all of the information from the owners table and the properties table.

apartmentlab=# SELECT * FROM owners
FULL OUTER JOIN properties
ON owners.id = properties.owner_id;
 id |  name  | age | id |   name    | num_units | owner_id 
----+--------+-----+----+-----------+-----------+----------
  1 | Donald |  56 |    |           |           |         
  2 | Elaine |  24 |    |           |           |         
  3 | Emma   |  36 |    |           |           |         
  4 | John   |  33 |    |           |           |         
    |        |     |  3 | Michelle  |        30 |         
    |        |     |  2 | Tara      |        50 |         
    |        |     |  1 | Archstone |        20 |  

Update at least one of your properties to belong to the owner with id 1.

apartmentlab=# UPDATE properties SET owner_id = 1
apartmentlab-# WHERE name = 'Archstone';
UPDATE 1
apartmentlab=# SELECT * FROM owners
FULL OUTER JOIN properties
ON owners.id = properties.owner_id;
 id |  name  | age | id |   name    | num_units | owner_id 
----+--------+-----+----+-----------+-----------+----------
  1 | Donald |  56 |  1 | Archstone |        20 |        1
  2 | Elaine |  24 |    |           |           |         
  3 | Emma   |  36 |    |           |           |         
  4 | John   |  33 |    |           |           |         
    |        |     |  3 | Michelle  |        30 |         
    |        |     |  2 | Tara      |        50 |         

Use an INNER JOIN to show all of the owners with associated properties.

apartmentlab=# SELECT * FROM owners 
apartmentlab-# INNER JOIN properties
apartmentlab-# ON owners.id = properties.owner_id;
 id |  name  | age | id |   name    | num_units | owner_id 
----+--------+-----+----+-----------+-----------+----------
  1 | Donald |  56 |  1 | Archstone |        20 |        1
(1 row)

Use a CROSS JOIN to show all possible combinations of owners and properties.

apartmentlab=# SELECT * FROM owners 
CROSS JOIN properties
WHERE owners.id = 1
apartmentlab-# ;
 id |  name  | age | id |   name    | num_units | owner_id 
----+--------+-----+----+-----------+-----------+----------
  1 | Donald |  56 |  2 | Tara      |        50 |         
  1 | Donald |  56 |  3 | Michelle  |        30 |         
  1 | Donald |  56 |  1 | Archstone |        20 |        1
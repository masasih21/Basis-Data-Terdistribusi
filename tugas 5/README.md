# Implementasi Cassandra Single Node

## Pendahuluan
apa itu database Cassandra dan apa perbedaannya dengan database relational dan database NoSQL lainnya.

## Arsitektur Server

## Instalasi Cassandra Single Node

## Dataset

## Import Dataset
```
CREATE KEYSPACE heroes WITH REPLICATION = {'class':'NetworkTopologyStrategy','datacenter1':1};
```

```
use heroes;
```

```
CREATE TABLE info (id int PRIMARY KEY, name text, gender text, eye_color text, race text, hair_color text, height float, publisher text, skin_color text, alignment text, weight float);
```

```
COPY info (id, name, gender, eye_color, race, hair_color, height, publisher, skin_color, alignment, weight) FROM '/vagrant/heroes.csv' WITH DELIMITER=',' AND HEADER=TRUE;
```
![copy1](screenshot/copy1.png)

```
SELECT * FROM info;
```
![read1](screenshot/read1.png)

![read3](screenshot/read3.png)

## CRUD Data
### a. Create Data
```
INSERT INTO info (id, name, gender, eye_color, race, hair_color, skin_color, alignment) VALUES (1000, 'ibuk', 'female', 'brown', 'human', 'black', 'langsat', 'good');
```

![insert](screenshot/insert.png)

### b. Read Data
```
SELECT * FROM info;
```

![read1](screenshot/read1.png)

### c. Update Data
```
UPDATE info SET height=150,weight=55 WHERE id=1000;
```

![update](screenshot/update.png)

### d. Delete Data
```
DELETE FROM info WHERE id=1000;
```

![delete](screenshot/delete.png)

## Referensi
https://www.digitalocean.com/community/tutorials/how-to-install-cassandra-and-run-a-single-node-cluster-on-ubuntu-14-04
https://docs.datastax.com/en/cql/3.3/cql/cql_reference/cqlCommandsTOC.html

# Implementasi Cassandra Single Node

## Pendahuluan
apa itu database Cassandra dan apa perbedaannya dengan database relational dan database NoSQL lainnya.

## 1. Arsitektur Server

## 2. Instalasi Cassandra Single Node
Untuk instalasi cassandra single node, ikuti langkah seperti pada [Instalasi Cassandra Single Node](https://github.com/masasih21/Basis-Data-Terdistribusi/tree/master/tugas%204/single-node).

## 3. Dataset
Dataset yang saya gunakan di sini adalah [Super Heroes Dataset](https://www.kaggle.com/claudiodavi/superhero-set#heroes_information.csv) yang berisi karakteristik pahlawan super.

Dataset ini memiliki 11 fitur yakni:
* ID
* Name : nama atau alias dari superhero
* Gender : jenis kelamin
* Eye Color : warna mata
* Race : ras
* Hair Color : warna rambut
* Height : tinggi badan (diukur dalam centimeter)
* Publisher : penerbit
* Skin Color : warna kulit
* Alignment : apakah termasuk superhero baik atau buruk
* Weight : berat badan (diukur dalam pound)

## 4. Import Dataset
Masuk pada command line cassandra
```
cqlsh
```

Membuat ```keyspace``` atau database pada cassandra dengan nama ```heroes```
```
CREATE KEYSPACE *keyspace_name* WITH REPLICATION = {'class':'*strategy*','*datacenter_name*':*N*};
```

```
CREATE KEYSPACE heroes WITH REPLICATION = {'class':'NetworkTopologyStrategy','datacenter1':1};
```

Menggunakan keyspace ```heroes```
```
use heroes;
```

Membuat tabel ```info``` dengan 10 fitur dan tipe data seperti berikut:
```
CREATE TABLE info (id int PRIMARY KEY, name text, gender text, eye_color text, race text, hair_color text, height float, publisher text, skin_color text, alignment text, weight float);
```

Mengimport dataset ```heroes.csv```
```
COPY info (id, name, gender, eye_color, race, hair_color, height, publisher, skin_color, alignment, weight) FROM '/vagrant/heroes.csv' WITH DELIMITER=',' AND HEADER=TRUE;
```
![copy1](screenshot/copy1.png)

*) tertulis Processed : 734 rows; yang berarti berhasil mengimport sebanyak 734 data.

Mengecek hasil import data
```
SELECT * FROM info;
```
![read1](screenshot/read1.png)

![read3](screenshot/read3.png)

## 5. CRUD Data
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
https://www.datastax.com/dev/blog/simple-data-importing-and-exporting-with-cassandra

# Implementasi Partisi

Sebelum melakukan testing partisi tabel, mengimport database baru pada node service
```
mysql -u root -p -t < /vagrant/test.sql
```
Untuk mengecek database telah terimport dengan baik pada node service yg ditunjuk, masuk pada mysql prompt:
```
mysql -u root -p
```
```
mysql> show databases;
```
![database](screenshot/database.png)
```
mysql> use test
mysql> show tables;
```
![table](screenshot/table.png)

Memberikan permission pada ```bdtuser``` agar dapat mengakses database dari luar
```
mysql> GRANT ALL PRIVILEGES on test.* to 'bdtuser'@'%';
mysql> FLUSH PRIVILEGES;
```

## 1. Mengecek Status Plugin Partition Aktif

Mengecek status dengan syntax berikut:
```
SHOW PLUGINS
```
atau dengan
```
SELECT
    PLUGIN_NAME as Name,
    PLUGIN_VERSION as Version,
    PLUGIN_STATUS as Status
    FROM INFORMATION_SCHEMA.PLUGINS
    WHERE PLUGIN_TYPE='STORAGE ENGINE';
```
![partition](screenshot/partition.png)

## 2. Membuat Partisi
### a. Range Partition
Pada range partition, data dikelompokkan berdasarkan range(rentang) nilai yang ditentukan. Range partition ini cocok digunakan pada kolom yang nilainya terdistribusi secara merata. Contoh yang paling sering adalah kolom tanggal.

### b. List Partition
Pada list partition, data dikelompokkan berdasarkan nilainya. Partisi ini cocok untuk kolom yang variasi nilainya tidak banyak.

### c. Hash Partition
Penentuan “nilai mana di taruh di partisi mana” dapat diatur secara internal berdasarkan hash value.

### d. Key Partition
Partisi ini didasarkan pada sebuah key dari tabel.

## 3. Testing pada "A Typical Use Case: Time Series Data"
### a. Melihat Plan Eksekusi
Pada tabel ```measures```
```
EXPLAIN SELECT *
FROM test.measures
WHERE measure_timestamp >= '2016-01-01' AND DAYOFWEEK(measure_timestamp) = 1;
```
![01](screenshot/01.png)

Pada tabel ```partititoned_measures```
```
EXPLAIN PARTITIONS SELECT *
FROM test.partitioned_measures
WHERE measure_timestamp >= '2016-01-01' AND DAYOFWEEK(measure_timestamp) = 1;
```
![02](screenshot/02.png)

### b. Select Queries Benchmark
Pada tabel ```measures```
```
SELECT SQL_NO_CACHE COUNT(*)
FROM test.measures
WHERE measure_timestamp >= '2016-01-01' AND DAYOFWEEK(measure_timestamp) = 1;
```
![03](screenshot/03.png)

Pata tabel ```partitioned_measures```
```
SELECT SQL_NO_CACHE COUNT(*)
FROM test.partitioned_measures
WHERE measure_timestamp >= '2016-01-01' AND DAYOFWEEK(measure_timestamp) = 1;
```
![04](screenshot/04.png)

Kesimpulan ...
### c. Big Delete Benchmark
Menambah data pada tabel ```measures``` dan ```partitioned_measures```
```
ALTER TABLE `test`.`measures` 
ADD INDEX `index1` (`measure_timestamp` ASC);

ALTER TABLE `test`.`partitioned_measures` 
ADD INDEX `index1` (`measure_timestamp` ASC);
```
![05](screenshot/05.png)
![06](screenshot/06.png)

Menghapus data pada tabel ```measures```
```
DELETE
FROM test.measures
WHERE  measure_timestamp < '2015-01-01';
```
![07](screenshot/07.png)

Menghapus data pada tabel ```partitioned_measures```
```
ALTER TABLE test.partitioned_measures 
DROP PARTITION to_delete_logs ;
```
![08](screenshot/08.png)

Menghapus data pada tabel ```measures```
```
DELETE
FROM test.measures
WHERE  measure_timestamp < '2016-01-01';
```
![09](screenshot/09.png)

Menghapus data pada tabel ```partitioned_measures```
```
ALTER TABLE test.partitioned_measures
DROP PARTITION prev_year_logs ;
```
![10](screenshot/10.png)

### Kesimpulan
...

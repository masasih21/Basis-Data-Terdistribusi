# Instalasi Cassandra Single Node

## Pendahuluan
Sistem ini terdiri dari sebuah Cassandra node. IP yang digunakan adalah ```192.168.33.11``` dengan hostname ```node1```.

## 1. Instalasi Oracle Java Virtual Machine
Untuk membuat paket Oracle JRE tersedia, menambahkan Personal Package Archives (PPA) menggunakan perintah berikut:
```
$ sudo add-apt-repository ppa:webupd8team/java
```
Mengupdate package yang telah ditambahkan
```
$ sudo apt-get update
```
Menginstall java
```
$ sudo apt-get install oracle-java8-set-default
```
Mengecek java yang telah terinstall
```
$ java -version
```
![java1](screenshot/java1.png)

## 2. Instalasi Cassandra
Menambahkan repositori cassandra ke ```/etc/apt/sources.list.d/cassandra.sources.list``` dengan perintah berikut:
```
$ echo "deb http://www.apache.org/dist/cassandra/debian 39x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
```
Menjalankan ```cURL``` untuk menambahkan kunci dari repositori
```
$ curl https://www.apache.org/dist/cassandra/KEYS | sudo apt-key add -
```
Mengupdate repositori
```
$ sudo apt-get update
```
Menginstall cassandra
```
$ sudo apt-get install cassandra
```
Mengecek status cassandra
```
$ sudo service cassandra status
```
![cass1](screenshot/cass1.png)

## 3. Menghubungkan ke Cluster
Setelah berhasil menginstall cassandra, periksa status cluster dengan perintah berikut:
```
$ sudo nodetool status
```
*UN berarti ```Up``` dan ```Normal```
![node1](screenshot/node1.png)

Menghubungkan dengan antarmuka command line ```cqlsh```
```
$ cqlsh
```
![cqlsh1](screenshot/cqlsh1.png)

## Referensi
https://www.digitalocean.com/community/tutorials/how-to-install-cassandra-and-run-a-single-node-cluster-on-ubuntu-14-04
https://www.liquidweb.com/kb/install-cassandra-ubuntu-16-04-lts/

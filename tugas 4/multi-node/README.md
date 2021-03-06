# Instalasi Cassandra Multi Node

## Pendahuluan
Sistem ini terdiri dari 2 buah Cassandra node. Berikut adalah pembagian IP beserta hostname yang digunakan:

| IP            | hostname  |
|---------------|-----------|
| 192.168.33.12	| node2     |
| 192.168.33.13	| node3     |

## 1. Instalasi Oracle Java Virtual Machine dan Cassandra
Untuk instalasi java virtual machine dan cassandra, ikuti langkah seperti pada [Instalasi Cassandra Single Node](https://github.com/masasih21/Basis-Data-Terdistribusi/tree/master/tugas%204/single-node).

### Status ```node2``` yang telah terinstall

Status java

![java2](screenshot/2java2.png)

Status cassandra

![cass2](screenshot/2cass2.png)

Status cluster

![node2](screenshot/2node2.png)

Tampilan antarmuka command line ```cqlsh```

![cqlsh2](screenshot/2cqlsh2.png)

### Status ```node3``` yang telah terinstall

Status java

![java3](screenshot/3java3.png)

Status cassandra

![cass3](screenshot/3cass3.png)

Status cluster

![node3](screenshot/3node3.png)

Tampilan antarmuka command line ```cqlsh```

![cqlsh3](screenshot/3cqlsh3.png)


## 2. Menghapus Data Default
Masuk pada cassandra node2 dan node3 (192.168.33.12, 192.168.33.13).

Sebelum menghapus data, mematikan masing-masing node cassandra terlebih dahulu
```
$ sudo service cassandra stop
```

Menghapus data default
```

$ sudo rm -rf /var/lib/cassandra/data/system/*
```

## 3. Konfigurasi Cluster
File konfigurasi Cassandra, ```cassandra.yaml``` terletak di direktori ```/etc/cassandra```. Pada langkah ini, akan dilakukan modifikasi file tersebut untuk mengatur cluster.

Membuka file ```cassandra.yaml``` untuk diedit
```
$ sudo nano /etc/cassandra/cassandra.yaml
```

Lakukan perubahan pada bagian-bagian berikut:
```
. . .
cluster_name: 'test'
. . .
seed_provider:
  - class_name: org.apache.cassandra.locator.SimpleSeedProvider
    parameters:
         - seeds: "192.168.33.12,192.168.33.13"
. . .
listen_address: 192.168.33.1#
. . .
rpc_address: 192.168.33.1#
. . .
endpoint_snitch: GossipingPropertyFileSnitch
. . .
```
Pada bagian bawah, tambahkan baris berikut:
```
auto_bootstrap: false
```

Keterangan:
* cluster_name: nama cluster
* -seeds: daftar alamat IP setiap node yang merupakan anggota cluster
* listen_address: alamat IP yang akan digunakan node lain dalam cluster untuk terhubung ke yang node sekarang ini
* rpc_address: alamat IP untuk panggilan prosedur jarak jauh
* endpoint_snitch: nama snitch, yang memberi tahu Cassandra tentang seperti apa jaringannya
* auto_bootstrap: membuat node baru secara otomatis menggunakan data yang benar

## 4. Konfigurasi Firewall

Untuk memungkinkan komunikasi, perlu membuka port jaringan berikut untuk setiap node:

```7000``` port TCP untuk perintah dan data.

```9042``` port TCP untuk server transportasi asli. ```cqlsh``` akan terhubung ke cluster melalui port ini.

Untuk memodifikasi firewall, buka rules file untuk IPv4.
```
$ sudo nano /etc/iptables/rules.v4
```

Menambahkan baris berikut :
```
$ -A INPUT -p tcp -s 192.168.33.1# -m multiport --dports 7000,9042 -m state --state NEW,ESTABLISHED -j ACCEPT
```

Merestart IPTables
```
$ sudo service iptables-persistent restart
```

## 5. Cek Status Cluster
Menjalankan masing-masing node cassandra
```
sudo service cassandra start
```

Mengecek status cluster
```
sudo nodetool status
```

Pada node2

![node2-2](screenshot/node2-2.png)

Pada node3

![node3-2](screenshot/node3-2.png)

Mengecek apakah terhubung atau tidak ke cluster menggunakan ```cqlsh```
```
$ cqlsh 192.168.33.1# 9042
```

Pada node2

![cqlsh2-1](screenshot/cqlsh2-1.png)

Pada node3

![cqlsh3-1](screenshot/cqlsh3-1.png)

## Referensi
https://www.digitalocean.com/community/tutorials/how-to-install-cassandra-and-run-a-single-node-cluster-on-ubuntu-14-04
https://www.digitalocean.com/community/tutorials/how-to-run-a-multi-node-cluster-database-with-cassandra-on-ubuntu-14-04

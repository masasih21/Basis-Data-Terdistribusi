# Registrasi dan Implementasi MongoDB

## Pendahuluan
Sebelum melakukan implementasi MongoDB, download file [MongoDB Community Server](https://www.mongodb.com/download-center/community?jmp=docs) lalu install.

## 1. Registrasi MongoDB Atlas
Kunjungi laman berikut : [sign up for MongoDB Atlas](https://www.mongodb.com/cloud/atlas)

Isi form sesuai dengan petunjuk lalu klik ```Get started free``` dan selesaikan proses pendaftaran.

![start](screenshot/start.png)

Jika berhasil maka akan tampil halaman berikut:

![cluster00](screenshot/cluster00.png)

## 2. Membuat Cluster MongoDB
Pada halaman tadi klik ```Build a Cluster```, akan muncul seperti berikut:

![cluster01](screenshot/cluster01.png)

Klik pada ```Create Cluster``` di pojok bawah

Hasilnya akan terbentuk ```Cluster0```

![cluster02](screenshot/cluster02.png)

Klik pada tab ```Security```

Pilih sub tab ```MongoDB Users```

Tambahkan user dengan mengklik ```ADD NEW USER```

![cluster03](screenshot/cluster03.png)

Mengisi ```username``` dan ```password```

![cluster04](screenshot/cluster04.png)

Klik ```add user```

Maka akan tampil hasil berikut:

![cluster05](screenshot/cluster05.png)

Pilih sub tab ```IP Whitelist```

Tambahkan IP dengan mengklik ```ADD IP ADDRESS```

![cluster06](screenshot/cluster06.png)

Akan tampil modal

![cluster07](screenshot/cluster07.png)

Pilih ```ADD CURRENT IP ADDRESS```

Secara otomatis akan ditambahkan IP komputer masing-masing

![cluster08](screenshot/cluster08.png)

Klik ```confirm```

Maka akan tampil hasil berikut:

![cluster09](screenshot/cluster09.png)

## 3. Mengimport Database
Database yang digunakan dalam tugas ini adalah database [emoji](https://www.kaggle.com/rtatman/emojinet)

Buka shell mongo, mengimport database
```
mongoimport --host cluster0-shard-00-00-ivehb.mongodb.net:27017 --db emoji --type json --file ~/Downloads/emojis.json --jsonArray --authenticationDatabase admin --ssl --username masasih21 --password 17102016
```
![import](screenshot/import.png)

Kembali pada laman MongoDB, jika berhasil maka akan terjadi perubahan pada ```Metric```

![hasil](screenshot/hasil.png)

## 4. Akses Data dengan Shell Mongo
Kembali pada laman MongoDB, pada cluster yang telah ada klik ```connect```

![connect](screenshot/connect.png)

Pilih ```Connect with the Mongo Shell```

Menyalin connection string yang telah disediakan

![shell01](screenshot/shell01.png)

Pada shell mongo, jalankan connection yang tadi telah dicopy
```
mongo "mongodb+srv://cluster0-ivehb.mongodb.net/test" --username masasih21
```

Isikan password

Maka akan muncul prompt:
```
MongoDB Enterprise Cluster0-shard-0:PRIMARY>
```

Untuk melihat daftar database yang telah dibuat
```
show databases
```
![shell02](screenshot/shell02.png)

Pindah ke database ```emoji```
```
use emoji
```
![shell03](screenshot/shell03.png)

Melihat koleksi dari database emoji
```
show collections
```
![shell04](screenshot/shell04.png)

Melihat data
```
db.emojis.find()
```
![shell05](screenshot/shell05.png)

## 5. Akses Data dengan MongoDB Compass
Membuka MongoDB Compass, berikut tapilan awal dari MongoDB Compass

![compass01](screenshot/compass01.png)

Kembali pada laman MongoDB, pada cluster yang telah ada klik ```connect```

![connect](screenshot/connect.png)

Pilih ```Connect with MongoDB Compass```

Menyalin connection string yang telah disediakan

![compass02](screenshot/compass02.png)

Kembali pada MongoDB Compass, connection string otomatis akan terdeteksi

![compass03](screenshot/compass03.png)

Klik ```Yes```

Jika benar maka akan tampil:

![compass04](screenshot/compass04.png)

Isikan ```password```, lalu klik ```CONNECT```

Jika sudah berhasil, muncul data database yang telah terbentuk pada cluster tersebut

![compass05](screenshot/compass05.png)

Klik pada database emoji, akan tampil collections dari database tersebut

![compass06](screenshot/compass06.png)

Klik pada collections maka akan tampil data

![compass07](screenshot/compass07.png)

## Referensi
https://docs.mongodb.com/manual/

https://www.youtube.com/watch?v=tpz-6Trd1UI

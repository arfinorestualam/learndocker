## Daftar Perintah Docker dan Kegunaannya

### Docker Version

- **Perintah**: `docker version`
- **Kegunaan**: Mengecek versi Docker yang terinstal, baik untuk client maupun server.

### Docker Image

#### Melihat Daftar Image
- **Perintah**: `docker image ls`
- **Kegunaan**: Menampilkan daftar image yang ada di sistem.

#### Mendownload Image
- **Perintah**: `docker image pull namaimage:tag`
- **Contoh**: `docker image pull redis:latest`
- **Kegunaan**: Mendownload image dari Docker registry.

#### Menghapus Image
- **Perintah**: `docker image rm namaimage:tag`
- **Contoh**: `docker image rm redis:latest`
- **Kegunaan**: Menghapus image dari sistem.

### Docker Container

#### Melihat Semua Container
- **Perintah**: `docker container ls -a`
- **Kegunaan**: Menampilkan semua container, termasuk yang tidak berjalan.

#### Melihat Container yang Sedang Berjalan
- **Perintah**: `docker container ls`
- **Kegunaan**: Menampilkan container yang sedang berjalan.

#### Membuat Container Baru
- **Perintah**: `docker container create --name namacontainer namaimage:tag`
- **Contoh**: `docker container create --name rediscontainer redis:latest`
- **Kegunaan**: Membuat container baru dari image.

#### Menjalankan Container
- **Perintah**: `docker container start containerId/namacontainer`
- **Contoh**: `docker container start redis1`
- **Kegunaan**: Menjalankan container yang sudah dibuat.

#### Menghentikan Container
- **Perintah**: `docker container stop containerId/namacontainer`
- **Contoh**: `docker container stop redis1`
- **Kegunaan**: Menghentikan container yang sedang berjalan.

#### Menghapus Container
- **Perintah**: `docker container rm containerId/namacontainer`
- **Contoh**: `docker container rm redis1`
- **Kegunaan**: Menghapus container yang sudah dihentikan.

### Docker Logs

#### Melihat Log Container
- **Perintah**: `docker container logs containerId/namacontainer`
- **Contoh**: `docker container logs redis1`
- **Kegunaan**: Menampilkan log dari container.

#### Melihat Log Secara Realtime
- **Perintah**: `docker container logs -f containerId/namacontainer`
- **Contoh**: `docker container logs -f redis1`
- **Kegunaan**: Menampilkan log secara realtime.

### Docker Exec

#### Menjalankan Perintah di Dalam Container
- **Perintah**: `docker container exec -i -t containerId/namacontainer /bin/bash`
- **Contoh**: `docker container exec -i -t redis1 /bin/bash`
- **Kegunaan**: Menjalankan perintah di dalam container, seperti membuka sesi shell.

### Port Forwarding

#### Membuka Port untuk Container
- **Perintah**: `docker container create --name containername --publish laptopport:containerport image:tag`
- **Contoh**: `docker container create --name nginx1 --publish 8080:80 nginx:latest`
- **Kegunaan**: Menghubungkan port di host dengan port di container.

### Environment Variables

#### Menambah Environment Variable
- **Perintah**: `docker container create --name containername --env KEY="value" --env KEY2="value" image:tag`
- **Contoh**: `docker container create --name mongo1 --publish 27017:27017 --env MONGO_INITDB_ROOT_USERNAME=fin --env MONGO_INITDB_ROOT_PASSWORD=fin mongo:latest`
- **Kegunaan**: Menambahkan environment variable ke container untuk konfigurasi aplikasi.

### Container Stats

- **Perintah**: `docker container stats`
- **Kegunaan**: Menampilkan penggunaan resource seperti CPU dan Memory untuk setiap container. Berguna untuk memantau container mana yang menggunakan resource secara berlebihan.

### Container Resource Limit

#### Membatasi Penggunaan Resource oleh Container
- **Perintah**: `docker container create --name namacontainer --publish port1:port2 --memory memorylimit --cpus cpulimit namaimage:tag`
- **Contoh**: `docker container create --name smallnginx --publish 8081:80 --memory 100m --cpus 0.5 nginx:latest`
- **Keterangan**:
    - `--memory`: Mengatur limit penggunaan memori. Contoh: `100m` untuk 100 MB, `1g` untuk 1 GB.
    - `--cpus`: Mengatur limit penggunaan CPU. Contoh: `0.5` untuk setengah core CPU.

### Bind Mounts

#### Menggunakan Bind Mounts untuk File Sharing
- **Perintah**: `docker container create --name namacontainer --mount "type=bind,source=folderhost,destination=foldercontainer,readonly" namaimage:tag`
- **Contoh**: `docker container create --name mongodata --mount "type=bind,source=/Users/khannedy/mongo-data,destination=/data/db" --publish 27018:27017 --env MONGO_INITDB_ROOT_USERNAME=fin --env MONGO_INITDB_ROOT_PASSWORD=fin mongo:latest`
- **Keterangan**:
    - `type`: Jenis mount, misalnya `bind` atau `volume`.
    - `source`: Lokasi file atau folder di sistem host.
    - `destination`: Lokasi file atau folder di container.
    - `readonly`: Opsi tambahan untuk membuat file/folder hanya dapat dibaca.

### Docker Volume

#### Kelebihan Docker Volume
- Docker volume direkomendasikan dibandingkan bind mounts karena:
  - Mendukung manipulasi volume secara langsung (membuat, melihat, dan menghapus volume).
  - Penyimpanan volume dikelola langsung oleh Docker, berbeda dengan bind mounts yang menggunakan penyimpanan host.

#### Melihat Daftar Volume
- **Perintah**: `docker volume ls`
- **Kegunaan**: Menampilkan daftar volume yang ada di Docker.

#### Membuat Volume Baru
- **Perintah**: `docker volume create namavolume`
- **Contoh**: `docker volume create redisvolume`
- **Kegunaan**: Membuat volume baru.

#### Menghapus Volume
- **Perintah**: `docker volume rm namavolume`
- **Contoh**: `docker volume rm redisvolume`
- **Kegunaan**: Menghapus volume yang tidak digunakan oleh container.

#### Container Volume (Menghubungkan Volume dengan Container)
- **Perintah**: `docker container create --name containername --mount "type=volume,source=namavolume,destination=foldercontainer,readonly" image:tag`
- **Contoh**: `docker container create --name mongocontainer --mount "type=volume,source=mongovolume,destination=/data/db" --publish 27018:27017 --env MONGO_INITDB_ROOT_USERNAME=fin --env MONGO_INITDB_ROOT_PASSWORD=fin mongo:latest`
- **Kegunaan**: Menghubungkan volume ke container agar data tetap aman meskipun container dihapus.

#### Backup Volume
- **Langkah Backup**:
  1. Matikan container yang volumenya ingin dibackup:  
     `docker container stop namacontainer`.
  2. Buat container dengan dua mount: satu bind mount untuk folder host, satu volume untuk data:  
     `docker container create --name backupcontainer --mount "type=bind,source=/backup,destination=/backup" --mount "type=volume,source=namavolume,destination=/data" image:tag`.
  3. Jalankan container dan lakukan backup dengan perintah:  
     `tar cvf /backup/backup.tar.gz /data`.

#### Restore Volume
- **Langkah Restore**:
  1. Buat volume baru untuk lokasi restore:  
     `docker volume create namavolume`.
  2. Buat container dengan dua mount: bind mount untuk file backup dan volume untuk restore:  
     `docker container create --name restorecontainer --mount "type=bind,source=/backup,destination=/backup" --mount "type=volume,source=namavolume,destination=/data" image:tag`.
  3. Jalankan container dan restore backup:  
     `tar xvf /backup/backup.tar.gz --strip 1`.

### Docker Network

#### Melihat Network
- **Perintah**: `docker network ls`
- **Kegunaan**: Melihat daftar network yang tersedia.

#### Membuat Network Baru
- **Perintah**: `docker network create --driver namadriver namanetwork`
- **Contoh**: `docker network create --driver bridge mynetwork`
- **Kegunaan**: Membuat network baru dengan driver tertentu (default: bridge).

#### Menghapus Network
- **Perintah**: `docker network rm namanetwork`
- **Contoh**: `docker network rm mynetwork`
- **Kegunaan**: Menghapus network yang tidak digunakan oleh container.

### Jaringan Container (Container Network)

- **Deskripsi**: Setelah membuat jaringan, kita dapat menambahkan container ke dalamnya. Jika beberapa container berada di dalam jaringan yang sama, mereka dapat saling berkomunikasi. Gunakan opsi `--network` saat membuat container untuk menentukan jaringan yang digunakan.

#### Menambahkan Container ke Jaringan
- **Perintah**: `docker container create --name namacontainer --network namajaringan image:tag`
- **Contoh**: `docker container create --name nginxnet --network nginxnetwork nginx:latest`
- **Penggunaan**: Membuat container dan menghubungkannya ke jaringan tertentu.

#### Komunikasi Antar Container
- Container yang berada di dalam jaringan yang sama dapat saling berkomunikasi menggunakan hostname, yaitu nama container.
- **Contoh Setup**:
    1. Buat jaringan:  
       `docker network create mongonetwork`
    2. Buat container MongoDB yang terhubung ke jaringan:  
       `docker container create --name mongodb --network mongonetwork --env MONGO_INITDB_ROOT_USERNAME=fin --env MONGO_INITDB_ROOT_PASSWORD=fin mongo:latest`
    3. Buat container MongoExpress yang terhubung ke jaringan:  
       `docker container create --name mongodbexpress --network mongonetwork --publish 8081:8081 --env ME_CONFIG_MONGODB_URL="mongodb://fin:fin@mongodb:27017/" mongo-express:latest`
    4. Jalankan kedua container:  
       `docker container start mongodb`  
       `docker container start mongodbexpress`
    5. Akses MongoExpress melalui `localhost:8081`. Container MongoExpress dapat berkomunikasi dengan MongoDB melalui jaringan.

#### Menghapus Container dari Jaringan
- **Perintah**: `docker network disconnect namajaringan namacontainer`
- **Contoh**: `docker network disconnect mongonetwork mongodb`
- **Penggunaan**: Melepaskan container dari jaringan.

#### Menambahkan Container ke Jaringan yang Sudah Ada
- **Perintah**: `docker network connect namajaringan namacontainer`
- **Contoh**: `docker network connect mongonetwork mongodb`
- **Penggunaan**: Menghubungkan container yang sudah dibuat ke jaringan yang ada.

### Inspect

- **Deskripsi**: Fitur `inspect` menyediakan informasi detail tentang objek Docker seperti image, container, volume, dan jaringan.

#### Memeriksa Image
- **Perintah**: `docker image inspect namagambar:tag`
- **Contoh**: `docker image inspect nginx:latest`
- **Penggunaan**: Melihat detail sebuah image, seperti variabel lingkungan dan port.

#### Memeriksa Container
- **Perintah**: `docker container inspect namacontainer`
- **Contoh**: `docker container inspect mongodb`
- **Penggunaan**: Melihat detail sebuah container, seperti volume dan jaringan.

#### Memeriksa Volume
- **Perintah**: `docker volume inspect namavolume`
- **Contoh**: `docker volume inspect mongodatarestore`
- **Penggunaan**: Melihat detail sebuah volume.

#### Memeriksa Jaringan
- **Perintah**: `docker network inspect namajaringan`
- **Contoh**: `docker network inspect mongonetwork`
- **Penggunaan**: Melihat detail sebuah jaringan.

### Prune

- **Deskripsi**: Fitur `prune` menghapus objek Docker yang tidak digunakan, seperti container yang berhenti, image yang tidak terpakai, serta jaringan atau volume yang tidak digunakan.

#### Menghapus Container yang Berhenti
- **Perintah**: `docker container prune`
- **Penggunaan**: Menghapus semua container yang sudah berhenti.

#### Menghapus Image yang Tidak Terpakai
- **Perintah**: `docker image prune`
- **Penggunaan**: Menghapus image yang tidak digunakan oleh container manapun.

#### Menghapus Jaringan yang Tidak Terpakai
- **Perintah**: `docker network prune`
- **Penggunaan**: Menghapus jaringan yang tidak digunakan oleh container manapun.

#### Menghapus Volume yang Tidak Terpakai
- **Perintah**: `docker volume prune`
- **Penggunaan**: Menghapus volume yang tidak digunakan oleh container manapun.

#### Menghapus Semua Objek yang Tidak Terpakai
- **Perintah**: `docker system prune`
- **Penggunaan**: Menghapus semua objek yang tidak digunakan (container, jaringan, cache, dan image, tetapi tidak termasuk volume).
- **Catatan**: Akan muncul prompt konfirmasi (`y`/`n`) sebelum eksekusi.

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


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


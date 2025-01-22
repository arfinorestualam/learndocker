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


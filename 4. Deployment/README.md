# Deployment

## The Goal

Pada tahap ini saya akan membuat deployment sebuah website bernama dumbmerch menggunakan docker dengan base image sekecil mungkin dan dapat berjalan dengan sempurna di lingkungan production.

## Frontend

Langkah awal anda dapat berpindah ke dalam branch production yang nantinya akan kita gunakan untuk set-up configurasi dan deployment pada website ini, anda dapat berpindah branch dengan comand :

```
git checkout production
```

![image](/4.%20Deployment/img/1.png)

Selanjutnya pada repositroy frontend anda bisa membuat `.dockerignore`

```
**/node_modules/
```
Baris ini memberi tahu Docker untuk mengabaikan direktori node_modules yang ditemukan di direktori saat ini atau salah satu subdirektorinya. Direktori node_modules biasanya berisi dependensi dari proyek Node.js, dan ukurannya bisa cukup besar dan tidak diperlukan untuk image Docker. Dengan mengabaikannya, Anda dapat mempercepat proses pembangunan Docker dan mengurangi ukuran dari image Docker yang dihasilkan.

Selanjutnya membuat `Dockerfile` pada repository frontend dengan distroless image :

```
# Set the base image to Node 16 (distroless)
FROM node:16 AS build

# Set the working directory inside the container
WORKDIR /app

# Copy all files from the current directory to the working directory in the container
COPY . .

# Install dependencies
RUN npm install

# Expose the port that the app will listen on
EXPOSE 3000

# Start the application
CMD ["yarn", "start"]
```

Untuk membuat menjadi sebuah docker image kita bisa menggunakan :

```
docker build -t sallfarr/fe-dumbmerch-production:latest .
```

![image](/4.%20Deployment/img/2.png)


### Backend

Sama seperti cara pertama kita bisa membuat `.dockerignore` :

```
.git
**/*.tar.gz
**/*.tgz
**/*.iso
**/*.raw
env/*/work-*
```

Script `.dockerignore` ini digunakan untuk memberi tahu Docker engine tentang file dan direktori mana yang harus diabaikan atau di-"ignore" ketika proses membangun image Docker dari direktori yang sama dengan file .dockerignore.

Selanjunya membuat `Dockerfile` pada directory backend :

```
FROM golang:1.16
RUN mkdir /app
COPY . /app
WORKDIR /app
RUN go get -x ./ && go build && go mod download
EXPOSE 5000
CMD ["go", "run", "main.go"]
```

Dan untuk membuat menjadi imagenya bisa menggunakan :

```
docker build -t sallfarr/be-dumbmerch-production:latest .
```

![image](/4.%20Deployment/img/3.png)

### Final Deployment

Agar mudah di manage dan bisa secara effisien menjalankan dan memberhentikan beberapa container yang ada kita bisa membuat `docker-compose`.

Disini saya akan membuat file `docker-compose.yml` dan menambahkan satu service container yang mana adalah databe postgresql, dan akan menjalankan image container yang sudah kita buat pada halaman awal.

```
version: "3.8"
services:
   db:
    image: postgres:latest
    container_name: db-dumbmerch
    ports:
      - 5432:5432
    volumes:
      - ~/postgresql:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=postgres
   backend:
    depends_on:
      - db
    image: sallfarr/be-dumbmerch-production:latest
    container_name: be-dumbmerch
    stdin_open: true
    restart: unless-stopped
    ports:
      - 5000:5000
   frontend:
    image: sallfarr/fe-dumbmerch-production:latest
    container_name: fe-dumbmerch
    stdin_open: true
    restart: unless-stopped
    ports:
      - 3000:3000
```

Untuk menjalankannya bisa menggunakan

```
docker-compose up -d
```

![image](/4.%20Deployment/img/4.png)

Registrasi user pada website

![image](/4.%20Deployment/img/5.png)

Dashboard profile dalam website dumbmerch

![image](/4.%20Deployment/img/6.png)































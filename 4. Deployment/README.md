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
FROM node:16 as build

# Set the working directory inside the container
WORKDIR /app

# Copy all files from the current directory to the working directory in the container
COPY . .

# Install dependencies
RUN npm install

# Expose the port that the app will listen on
EXPOSE 3000

# Start the application
CMD ["node", "node_modules/serve/bin/serve.js", "build", "-l", "3000"]
```

Untuk membuat menjadi sebuah docker image kita bisa menggunakan :

```
docker build -t sallfarr/fe-dumbmerch-production:latest .
```

![image](/4.%20Deployment/media/2.png)


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
# Build Stage
FROM golang:1.18 AS build

WORKDIR /go/src/app
COPY . .

RUN go mod download
RUN CGO_ENABLED=0 go build -o /go/bin/app

# Final Stage
FROM gcr.io/distroless/static

COPY --from=build /go/bin/app /

CMD ["/app"]
```

Dan untuk membuat menjadi imagenya bisa menggunakan :

```
docker build -t sallfarr/be-dumbmerch-production:latest .
```

### Final Deployment

Agar mudah di manage dan bisa secara effisien menjalankan dan memberhentikan beberapa container yang ada kita bisa membuat `docker-compose`.

Disini saya akan membuat file `docker-compose.yml` dan menambahkan satu service container yang mana adalah databe postgresql, dan akan menjalankan image container yang sudah kita buat pada halaman awal.

```
version: "3.8"

services:
  db:
    build:
      context: .
      dockerfile: Dockerfile.postgres  # Use the custom Distroless Dockerfile for the PostgreSQL service
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

# Define a custom bridge network for the containers to communicate with each other
networks:
  default:
    driver: bridge
```

[Dockerfile.postgress](resource/Dockerfile.postgres)

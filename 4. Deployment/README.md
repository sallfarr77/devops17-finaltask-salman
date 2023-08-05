# Deployment

## The Goal

Pada tahap ini saya akan membuat deployment sebuah website bernama dumbmerch menggunakan docker dengan base image sekecil mungkin dan dapat berjalan dengan sempurna di lingkungan production.

## Frontend

Langkah awal anda dapat berpindah ke dalam branch production yang nantinya akan kita gunakan untuk set-up configurasi dan deployment pada website ini, anda dapat berpindah branch dengan comand :

```
git checkout production
```

Selanjutnya pada repositroy frontend anda bisa membuat `.dockerignore`

```
**/node_modules/
```
Baris ini memberi tahu Docker untuk mengabaikan direktori node_modules yang ditemukan di direktori saat ini atau salah satu subdirektorinya. Direktori node_modules biasanya berisi dependensi dari proyek Node.js, dan ukurannya bisa cukup besar dan tidak diperlukan untuk image Docker. Dengan mengabaikannya, Anda dapat mempercepat proses pembangunan Docker dan mengurangi ukuran dari image Docker yang dihasilkan.

Selanjutnya membuat `Dockerfile` pada repository frontend dengan distroless image :

```
# Use a distroless base image for Node.js
FROM gcr.io/distroless/nodejs:16

WORKDIR /app

# Copy only the necessary application files
COPY package.json .
COPY package-lock.json .
COPY .env .
COPY src/ ./src/

# Install production dependencies only
RUN npm install --only=production

EXPOSE 3000

# Start the application
CMD [ "/app/src/index.js" ]
```

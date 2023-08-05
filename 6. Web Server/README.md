# Web Server

## Cloudflare

Untuk setup dns disini saya menggunakan cloudflare dengan semua alamat ip mengarah ke gateway server.

![image](/6.%20Web%20Server/img/1.png)

## SSL Certbot using wildcard

Sebelum memasang ssl kita bisa masuk dulu kedalam server gateway dan pada kesempatan kali ini saya akan memasang ssl dengan bantuan certbot.

Sebelumnya bisa menginstall cerbot terlebih dahulu dengan

```
sudo snap install --classic certbot
```

Selanjutnya setup credentials, sebagai contoh :

```
sudo certbot certonly \
--manual \
--preferred-challenges=dns \
--email sallfarr@outlook.com \
--server https://acme-v02.api.letsencrypt.org/directory \
--agree-tos \
-d *.studentdumbways.my.id
```

![image](/6.%20Web%20Server/img/2.png)

Disini kita akan menyelesaikan chellange

![image](/6.%20Web%20Server/img/3.png)

Setup challange menggunakan cloudflare, jika sudah save

![image](/6.%20Web%20Server/img/4.png)

Challange berhasil

Dan untuk renew cert secara otomasti bisa coba menggunakan :

```
sudo certbot renew --dry-run
```

## Open https website

![image](/6.%20Web%20Server/img/5.png)

![image](/6.%20Web%20Server/img/6.png)

![image](/6.%20Web%20Server/img/7.png)

![image](/6.%20Web%20Server/img/8.png)
































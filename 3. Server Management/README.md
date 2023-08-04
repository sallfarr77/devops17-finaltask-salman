# Server Management

## Disable password login

Mematikan cara login menggunakan password sangat berguna untuk ke amanan server kita, karna pada pasalnya kita telah menutup salah satu pintu dari celah kerentanan pada server yang mana hanya ada satu cara untuk masuk kedalam server yaitu memiliki private-key.

Karna pada tahap provisioning kita telah menonaktifkan semua login menggunakan password dengan menggunakan script ansible maka dari itu kita tidak perlu mematikannya lagi secara manual.

![image](/3.%20Server%20Management/img/1.png)

Dan beginilah hasilnya jika kita ingin masuk kedalam server akan tetapi kita tidak memiliki private-key, server secara otomatis akan langsung menolak tanpa menawarkan login menggunakan password.

![image](/3.%20Server%20Management/img/2.png)

## One gateway

Selanjutnya saya akan membuat sistem one gateway, yang mana kita bisa mengakses semua server dengan perantara server gateway.

Dan untuk caranya sendiri kita bisa membuat file `config` didalam directory `.ssh` seperti ini

![image](/3.%20Server%20Management/img/3.png)

Jika suda sekarang kita bisa masuk ke server tujuan dengan cara :

```
ssh <server-hostname>
```
![image](/3.%20Server%20Management/img/4.png)


## Firewall

Untuk langkah terakhir kita akan membuat configurasi firewall menggunakan ansible dengan target server gateway, yang mana dengan firewall ini kita hanya akan mengizinkan dan memperbolehkan akses dari port yang sudah kita set dan beri izin.

Disini saya akan memberi nama file ini `set-firewall.yml`.

![image](/3.%20Server%20Management/img/5.png)

![image](/3.%20Server%20Management/img/6.png)











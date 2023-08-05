# Monitoring

Setelah berhasil mendeploy aplikasi, tugas kita selanjutnya adalah memonitoring server agar jika ada masalah kita bisa bertindak secara cepat untuk memperbaiki masalah tersebut.

## Node-exporter

Hal yang pertama sebelum memonitoring server kita perlu yang namanya data, nah data ini bisa kita ambil dengan menggunakan node-exporter, jadi secara simple node-exporter berguna untuk meng scrape-data.

![image](/5.%20Monitoring/img/1.png)

## Prometheus

Selanjutnya ada prometheus, jika tadi node-exporter berguna sebagai meng scrape-data yang ada di server makan prometheus berguna sebagai wadah atau pengelola semua data yang suda di scrape.

![image](/5.%20Monitoring/img/2.png)

## Grafana

Yang terakhir ada grafana, nah jadi grafana ini berperan untuk mengubah atau men-visualisi semua data yang ada di server menjadi GUI yang mudah dibaca.

Untuk set-upnya sendiri bisa mengikuti seperti gambar dibawah ini :

Masuk kedalam dashboard grafana dan pilih `add data-source` selanjutnya kita bisa menggunakan data yang ada di prometheus.

![image](/5.%20Monitoring/img/3.png)

Jika data sudah di tambah anda bisa langsung membuat dashboard untuk monitoring sesuai dengan kebutuhan dengan selera, berikut adalah dashboard yang telah selesai dibuat.

![image](/5.%20Monitoring/img/4.png)

Nah dari sini kita bisa memantau seberapa besar resource yang terpakai didalam server kita, dan bisa bertindak cepat jikalau nanti ada kejanggalan di dalam server yang telah kita buat.

Anda juga bisa membuat sistem alert yang nantinya si grafana akan memberitahu kita entah itu melalui telegram atau discord jikalau server kita telah melebihi kapasitas resource tertentu.

Sebagai contoh disini saya membuat contact point lewat discord, yang mana nanti grafana akan secara otomatis memberikan kita pesan dan pesan peringatan, untu api nya kalian bisa coppy-paste dari webbook-url yang ada didalam server discord kalian.

![image](/5.%20Monitoring/img/5.png)

![image](/5.%20Monitoring/img/6.png)

![image](/5.%20Monitoring/img/7.png)








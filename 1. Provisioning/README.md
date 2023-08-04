# Provision servers using Terraform


Sebagai lankgkah awal saya akan membuat file `main.tf`, yang nantinya akan membuat resource vm berbeda dengan menerapkan IaC menggunakan terraform.

```
terraform {
  required_providers {
    idcloudhost = {
      source = "bapung/idcloudhost"
      version = "0.1.3"
    }
  }
}

provider "idcloudhost" {
  auth_token = ""
}

resource "idcloudhost_vm" "appserver" {
  name = "salman-appserver"
  os_name = "ubuntu"
  os_version = "20.04"
  vcpu = 2
  memory = 2048
  disks = 20
  username = "sallfarr"
  initial_password = ""
  public_key = ""
  billing_account_id = ""
}

resource "idcloudhost_vm" "gateway" {
  name = "salman-gateway"
  os_name = "ubuntu"
  os_version = "20.04"
  vcpu = 2
  memory = 2048
  disks = 20
  username = "sallfarr"
  initial_password = ""
  public_key = ""
  billing_account_id = ""
}

resource "idcloudhost_vm" "monitoring" {
  name = "salman-monitoring"
  os_name = "ubuntu"
  os_version = "20.04"
  vcpu = 2
  memory = 1024
  disks = 20
  username = "sallfarr"
  initial_password = ""
  public_key = ""
  billing_account_id = ""
}

resource "idcloudhost_floating_ip" "appserver" {
  name = "appserverIP"
  billing_account_id = 
  assigned_to = idcloudhost_vm.appserver.id
}

resource "idcloudhost_floating_ip" "ipgateway" {
  name = "gatewayIP"
  billing_account_id = 
  assigned_to = idcloudhost_vm.gateway.id
}

resource "idcloudhost_floating_ip" "ipmonitoring" {
  name = "monitoringIP"
  billing_account_id = 
  assigned_to = idcloudhost_vm.monitoring.id
}
```

Sebagai lankah awal kita bisa menggunakan terrafrom init untuk menginisialisasi konfigurasi proyek Terraform :

```
terraform init
```
![image](/1.%20Provisioning/img/1.png)

Kita juga bisa menggunakan terraform plan yang digunakan untuk menganalisis dan menampilkan rencana perubahan infrastruktur yang akan dilakukan :

```
terraform plan
```

![image](/1.%20Provisioning/img/2.png)

terraform apply adalah perintah dalam Terraform yang digunakan untuk menerapkan perubahan infrastruktur yang telah didefinisikan dalam konfigurasi Terraform :

```
terraform apply
```
![image](/1.%20Provisioning/img/3.png)

![image](/1.%20Provisioning/img/4.png)


# Server Configuration using Ansible

Disini saya akan membuat ansible.cfg, ansible.cfg adalah file konfigurasi utama untuk Ansible, yang digunakan untuk mengonfigurasi berbagai aspek penggunaan Ansible. File ini digunakan untuk menentukan opsi dan preferensi yang akan digunakan oleh Ansible saat menjalankan perintah dan tugas.

```
[defaults]
inventory = Inventory
private_key_file = ~/.ssh/id_rsa
host_key_checking = false
interpreter_python = auto_silent
```

Dan Jangakn lupa pula membuat Inventory, Inventory Ansible adalah file atau grup file yang digunakan untuk menyimpan daftar host atau target yang akan dikelola oleh Ansible. File inventory ini berisi informasi tentang host seperti alamat IP, nama host, username SSH, grup host, dan variabel lainnya yang relevan. Saat menjalankan perintah Ansible atau playbook, Anda akan menyebutkan file inventory yang akan digunakan.

```
[appserver]
103.172.205.204

[gateway]
116.193.190.5

[monitoring]
103.172.204.221

[all:vars]
ansible_user=sallfarr
```

Selanjutnya saya akan membuat file ansible-playbook.yml, ansible-playbook.yml adalah file yang digunakan untuk menulis playbook Ansible. Playbook adalah file konfigurasi utama yang mendefinisikan tugas-tugas yang harus dilakukan oleh Ansible pada target atau host tertentu. File ini berisi langkah-langkah yang akan dijalankan oleh Ansible untuk mencapai tujuan tertentu, seperti mengatur konfigurasi, menginstal perangkat lunak, atau mengelola sumber daya.

Saya akan membuat file ansible-playbook dengan nama play.yml, File `play.yml` merupakan sebuah playbook Ansible yang berisi serangkaian tugas untuk mengotomatisasi konfigurasi sistem, instalasi Docker, dan penyiapan Jenkins, Prometheus, dan Grafana menggunakan kontainer Docker.

- Pada bagian pertama, playbook ini akan mengkonfigurasi otentikasi SSH menggunakan kunci publik, mengubah sumber repository APT, dan menonaktifkan opsi PasswordAuthentication untuk SSH login.
- Selanjutnya, pada bagian kedua, playbook ini akan menginstal Docker dan dependensinya pada semua host yang ada.
- Pada bagian ketiga, playbook ini akan mendeploy Jenkins, Prometheus, dan Grafana di host yang ditandai sebagai "monitoring" dengan menggunakan kontainer Docker.

Playbook ini mempermudah proses konfigurasi dan deploy perangkat lunak tersebut secara konsisten pada beberapa host, sehingga memastikan kehandalan sistem dan monitoring yang efisien.

Klik disini untuk bisa melihat code secara lebih lengkap: [play.yml](https://github.com/sallfarr77/devops17-finaltask-salman/blob/main/resource/play.yml)

Dan untuk menjankannya bisa menggunakan :

```
ansible-playbook play.yml
```

Selanjutnya setup reverse proxy sesuai dengan keinginan, untuk contohnya kurang lebih seperti ini [reverse-proxy.conf](https://github.com/sallfarr77/devops17-finaltask-salman/blob/main/resource/reverser-proxy.conf)

Jika sudah kita bisa membuat file ansible-playbook yang didalamnya menginstall nginx dan menjalankan configurasi reverse proxy yang tadi sudah kita buat :

```
- hosts: gateway
  become: true
  tasks:
    - name: Installing nginx
      apt:
        name: nginx
        state: latest
        update_cache: yes

    - name: Start nginx
      service:
        name: nginx
        state: started

    - name: Copy reverse-proxy
      copy:
        src: ~/ansible-server/reverse-proxy.conf
        dest: /etc/nginx/sites-enabled

    - name: Reload nginx
      service:
        name: nginx
        state: reloaded
```

dan untuk hasilnya :

![image](/1.%20Provisioning/img/5.png)
















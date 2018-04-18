![](https://blog.theodo.fr/wp-content/uploads/2017/07/Vagrant.png)

## Soal Sesilab Nginx

Buatlah sistem load balancing dengan 1 load balancer(nginx dan 2 worker(apache2), terapkan algoritma load balancing round-robin, least-connected, dan ip-hash.

Soal :

1. Buatlah Vagrantfile sekaligus provisioning-nya untuk menyelesaikan kasus.
2. Analisa apa perbedaan antara ketiga algoritma tersebut.
3. Biasanya pada saat membuat website, data user yang sedang login disimpan pada session. Sesision secara default tersimpan pada memory pada sebuah host. Bagaimana cara mengatasi masalah session ketika kita melakukan load balancing?

### Petunjuk

1. Lakukan soal nomor 1 dan dokumentasikan bagaimana cara setupnya pada laporan markdown.
2. Untuk nomor 2 dan 3 merupakan analisa terhadap suatu masalah, jawablah pertanyaan diatas dan tulis pada laporan.


## 1. Langkah-langkah
### a. Buat 3 Vagrant (1 Load Balancer + 2 Worker)
	- Buat 3 folder untuk Vagrant
		$ mkdir loadbalancer1 worker1 worker

	- Masuk ke folder loadbalancer1
		$ cd loadbalancer1

	- Inisiasi Project Vagrant
		$ vagrant init

	- Tambakan box dengan OS Ubuntu 16.04
		$ vagrant box add ubuntu/xenial64

	- Edit file Vagrantfile
		$ sudo nano Vagrantfile

	- Ubah bagian
		config.vm.box = "base"	
		menjadi	
		config.vm.box = "ubuntu/xenial64"



### b. Buat Provision untuk Instalasi Software Load Balancer dan Worker
	- Di masing-masing folder, buat file baru bootstrap.sh
		$ sudo nano bootstrap.sh

	- Isikan masing-masing file bootstrap.sh sebagai berikut:
		- Load Balancer : 
			> #!/usr/bin/env bash
			> apt-get install nginx -y
			> apt-get install php7.0-fpm -y

		- Worker :
			> #!/usr/bin/env bash
			> apt-get install apache2
			> apt-get install libapache2-mod-php7.0 php7.0-fpm -y


### c. Setup Algoritma yang ingin digunakan di Load Balancer
	- Load Balancer :
		
		./load_balancing start [balancing_method]
			
		Balancing method diisi dengan algoritmanya, 
		contoh:
		
		./load_balancing start round_robin
		./load_balancing start ip_hash
		./load_balancing start least_conn
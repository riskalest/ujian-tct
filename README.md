Dockerfile
==========

## *Apa itu Dockerfile?*

Dockerfile sendiri adalah merupakan skrip yang berisi atau terdiri dari serangkaian perintah, intruksi (argumen) yang akan dieksekusi secara otomatisasi dan berurutan untuk membangun sebuah image. Jadi akan mempermudah kita untuk tidak selalu mengetikkan perintah setiap kali kita akan menjalankan container.

## *Contoh penggunaan Dockerfile*

Contoh penggunaan dockerfile adalah untuk membuat image.*( A Dockerfile is a recipe (or blueprint if that helps) for building Docker images, and the act of running a separate build command produces the Docker image from that recipe.)*
 
*1. Dockerfile Commands*

Sebelum kita memulai menggunakan dockerfile untuk membuat sebuah container, kita sedikit belajar dulu tentang perintah command yang ada pada dockerfile.

ADD
Perintah ADD digunakan untuk mengcopy file dari suatu direktori ke direktori tujuan.
Jika direktori asal adalah sebuah URL, perintah add akan mendownloadnya dan menempatkannya ke direktori tujuan.

CMD
Perintah CMD hampir sama dengan perintah RUN, CMD digunakan untuk mengeksekusi perintah yang lebih spesifik, seperti pada saat proses pembuatan container pada image.

ENTRYPOINT
ENTRYPOINT adalah argumen untuk mengeset default aplikasi yang digunakan setiap kali sebuah container dibuat menggunakan image.

ENV
ENV digunakan untuk mengeset environment variables.

FROM
FROM argument mendefinisikan sebuah base image yang akan digunakan untuk memulai membangun proses pada setiap docker image apakah itu di repositori ataupun di host kita sendiri.

WORKDIR
WORKDIR direktif digunakan untuk mengatur di mana perintah didefinisikan dengan CMD yang akan dieksekusi.

RUN
RUN adalah perintah yang digunakan untuk membangun docker images yang terpusat untuk mengeksekusi Dockerfiles.

MAINTAINER
MAINTAINER adalah perintah yang tidak dijalankan tetapi di deklarasikan sebagai author field dari images.

USER
USER direktif digunakan untuk mengatur UID (atau nama pengguna) yang menjalankan sebuah container berdasarkan dari image yang sedang dibangun.

VOLUME
Perintah VOLUME digunakan untuk mengaktifkan akses dari kontainer kita ke direktori pada mesin host.

EXPOSE
Perintah EXPOSE digunakan untuk menghubungkan port tertentu untuk mengaktifkan network antara proses yang berjalan di dalam container dan mesin host.
 
 
*2. Membuat Dockerfile dan Build Image*

Sekarang kita langsung saja mencoba membuat dockerfile untuk membuat suatu image yang berisi container. Sebagai contoh disini saya akan membuat container yang berisi Nginx web server. Pertama buat dulu file dockerfile dengan editor yang biasa anda gunakan, disini saya menggunakan vi

    vi Dockerfile

Kemudian kita tambahkan konfigurasi docker yang didalamnya terdapat perintah dan argument untuk membuat container Nginx web server. Saya akan menggunakan sebuah base image ubuntu. Lebih lengkapnya tambhakan konfigurasi dibawah ini

    # Pull base image.
    FROM ubuntu:14.04

    RUN apt-get install -y –no-install-recommends software-properties-common

    # Install Nginx.
    RUN \
    add-apt-repository -y ppa:nginx/stable && \
    apt-get update && \
    apt-get install -y nginx && \
    rm -rf /var/lib/apt/lists/* && \
    echo “\ndaemon off;” >> /etc/nginx/nginx.conf && \
    chown -R www-data:www-data /var/lib/nginx

    # Define mountable directories.
    VOLUME [“/etc/nginx/sites-enabled”, “/etc/nginx/certs”, “/etc/nginx/conf.d”, “/var/log/nginx”, “/var/www/html”]

    # Define working directory.
    WORKDIR /etc/nginx

    # Define default command.
    CMD [“nginx”]

    # Expose ports.
    EXPOSE 80
    EXPOSE 443

Anda dapat melihat dengan perintah EXPOSE, saya menghubungkan dan mengaktifkan port http dan https antara container dan mesin host. Save file diatas dan keluar dari editor.

Langsung saja kita build image nya dengan menggunakan dockerfile diatas, untuk mulai membuild jalankan perintah berikut

    docker build -t nginx_webserver .

 ![logo](https://github.com/riskalest/ujian-tct/blob/master/1.jpg)	
	
Cek hasilnya dengan perintah

    docker images

 ![logo](https://github.com/riskalest/ujian-tct/blob/master/2.jpg)
	
disitu terlihat dengan image ID 5d62ac5f7514
	
 
#### Terimakasih :)


REFERENSI :
===========
https://andykamt.com/belajar-docker-membuat-container-menggunakan-dockerfile/
https://nickjanetakis.com/blog/differences-between-a-dockerfile-docker-image-and-docker-container

# TKA-Kelompok-B5
### Anggota Kelompok B5 :
- Farida Qurrotu A'yuna 5027231015

- Tsaldia Hukma Cita 5027231036

- Rafika Az Zahra Kusumastuti 5027231050

- Radella Chesa Syaharani 5027231064

- Syela Zeruya Tandi Lalong 5027231076
  
---

## Daftar Isi

### Study Case

Anda adalah seorang lulusan Teknologi Informasi, sebagai ahli IT, salah satu kemampuan yang harus dimiliki adalah Keampuan merancang, membangun, mengelola aplikasi berbasis komputer menggunakan layanan awan untuk memenuhi kebutuhan organisasi.

Pada suatu saat anda mendapatkan project untuk mendeploy sebuah aplikasi Sentiment Analysis dengan komponen Backend menggunakan python: sentiment-analysis.py dengan spesifikasi sebagai berikut

---

### Endpoint

  1. Analyze Text

      - Endpoint: POST /analyze
        
      - Description: This endpoint accepts a text input and returns the sentiment score of the text.

      - Request:

            {
               "text": "Your text here"
            }

      - Response:
      
            {
              "sentiment": <sentiment_score>
            }

  2. Retrieve History

      - Endpoint: GET /history
     
      - Description: This endpoint retrieves the history of previously analyzed texts along with their sentiment scores.

      - Response:
        
            {
             {
               "text": "Your previous text here",
               "sentiment": <sentiment_score>
             },
             ...
            }

Kemudian juga disediakan sebuah Frontend sederhana menggunakan index.html dan styles.css dengan tampilan antarmuka sebagai berikut:

<img width="1186" alt="image" src="https://github.com/lasturas/TKA-Kelompok-B5/assets/151950309/38169a9b-d24e-457d-aac3-21cf5159d67c">

Kemudian anda diminta untuk mendesain arsitektur cloud yang sesuai dengan kebutuhan aplikasi tersebut. Apabila dana maksimal yang diberikan adalah 1 juta rupiah per bulan (65 US$) konfigurasi cloud terbaik seperti apa yang bisa dibuat?

---

## Hasil Final Project

### Rancangan Arsitektur

![image](https://github.com/lasturas/TKA-Kelompok-B5/assets/151950309/1bec5696-fbb1-4426-9d7d-566839c1c72d)

### Tabel Harga

|NO|Nama|Spesifikasi|Fungsi|Harga/Bulan|
|--|--|--|--|--|
|1|TKAB5-VM1|1 Intel vCPU / 2GB Memory / 50GB Disk SSD / SGP1 - Ubuntu 24.04 (LTS) x64|App Worker 1|12$| 
|2|TKAB5-VM2|1 Intel vCPU / 2GB Memory / 50GB Disk SSD / SGP1 - Ubuntu 24.04 (LTS) x64|App Worker 2|12$| 
|3|TKAB5-VM3-DB|2 Intel vCPU / 2GB Memory / 60GB Disk SSD / SGP1 - Ubuntu 24.04 (LTS) x64|Mongo Database|18$| 
|4|TKAB5-VM4-LOAD|2 Intel vCPU / 2GB Memory / 60GB Disk SSD / SGP1 - Ubuntu 24.04 (LTS) x64|Load Balancer (Round Robin)|18$|

### Cara Pengerjaan
### Database

1. Buka terminal windows dan hubungkan ke terminal VM

       ssh root@152.42.238.180
   ![image](https://github.com/lasturas/TKA-Kelompok-B5/assets/151950309/79723b83-1a53-4516-b123-f44a37d399ba)

2. Install MongoDB
   
        sudo apt update
        sudo apt upgrade
    
        # Install Dependency
        sudo apt install gnupg wget apt-transport-https ca-certificates software-properties-common
        echo "deb http://security.ubuntu.com/ubuntu focal-security main" | sudo tee /etc/apt/sources.list.d/focal-security.list
        sudo apt-get update
        sudo apt-get install libssl1.1
    
        # Install MongoDB
        curl -fsSL https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
        echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
        sudo apt update
        sudo apt install mongodb-org

3. Akses MongoDB

       sudo systemctl start mongod
       sudo systemctl enable mongod

4. Konfigurasi MongoDB

       sudo nano /etc/mongod.conf
   
   
5. Restart MongoDB   

       sudo systemctl restart mongod


6. Buka port Firewall

       sudo ufw allow <<PORT>>

7. Buka shell MongoDB

       mongo

8. Masuk sebagai `Admin`

       use admin

9. Buat user `Admin`

       db.createUser({
       user: "KelompokTKAB5",
       pwd: "KelompokTKAB5",
       roles: [{ role: "userAdminAnyDatabase", db: "admin" }]
       })

10. Check user yang dibuat

        db.getUser("KelompokTKAB5")

11. Sambungkan ke MongoDBCompass

        mongodb://146.190.102.47:<<PORT>>

12. Konfigurasi database selesai/berhasil ketika sudah bisa terhubung dengan Compass.



### Worker (VM-1 & VM-2)

1. Sambungkan terminal Windows dengan terminal VM

2. Download semua resource yang diperlukan

       << GITHUB >>

3. Install `Nginx`

       sudo apt update
       sudo apt upgrade -y
       sudo apt install nginx -y

4. Install dependensi python

       sudo apt update
       sudo apt install python3 -y
       sudo apt install python3-pip -y
       sudo apt install python3.12-venv

       python3 -m venv myenv
       source myenv/bin/activate

       pip install flask
       pip install flask_cors
       pip install gunicorn
       pip install flask_pymongo
       pip install textblob
       pip install pymongo
       pip install gevent

5. Pindah file `index.html` kedalam `/var/www/html`

       mv index.html /var/www/html/index.html

6. Fetch pada index.html agar mengarah ke ip worker

7. Konfigurasi /etc/nginx/sites-enabled/default

   Tambahkan route ke endpoint /analyze dan /history

8. Konfigurasi IP Database pada `sentiment-analysis.py` agar tersambung

9. Lalu restart Nginx

        bash sudo service nginx restart

10. Run `sentiment-analysis.py`

11. Lakukan testing dengan query untuk memastikan apakah bisa berjalan dengan lancar


### Load-Balancer 

1. Sambungkan terminal VM

2. Install Nginx

       sudo apt update
       sudo apt upgrade -y
       sudo apt install nginx -y

3. Konfigurasi file `default` pada `/etc/nginx/sites-enabled/default`

4. Restart Nginx

       sudo sevice nginx restart

5. Refresh page berkali-kali untuk test load-balancer





# Jarkom-Modul-1-B10-2021

Anggota B10:
1. Pramudya Tiandana Wisnu Gautama 05111940000018
2. Jason Andrew Gunawan 05111940000085
3. Frans Wijaya 05111940000098

## SOAL
1. Sebutkan web server yang digunakan pada "ichimarumaru.tech"! 
2. Temukan paket dari web-web yang menggunakan basic authentication method!
3. Ikuti perintah di basic.ichimarumaru.tech! Username dan password bisa didapatkan dari file .pcapng!
4. Temukan paket mysql yang mengandung perintah query select!
5. Login ke portal.ichimarumaru.tech kemudian ikuti perintahnya! Username dan password bisa didapat dari query insert pada table users dari file .pcap!
6. Cari username dan password ketika melakukan login ke FTP Server!
7. Ada 500 file zip yang disimpan ke FTP Server dengan nama 0.zip, 1.zip, 2.zip, ..., 499.zip. Simpan dan Buka file pdf tersebut. (Hint = nama pdf-nya "Real.pdf")
8. Cari paket yang menunjukan pengambilan file dari FTP tersebut!
9. Dari paket-paket yang menuju FTP terdapat indikasi penyimpanan beberapa file. Salah satunya adalah sebuah file berisi data rahasia dengan nama "secret.zip". Simpan dan buka file tersebut!
10. Selain itu terdapat "history.txt" yang kemungkinan berisi history bash server tersebut! Gunakan isi dari "history.txt" untuk menemukan password untuk membuka file rahasia yang ada di "secret.zip"!
11. Filter sehingga wireshark hanya mengambil paket yang berasal dari port 80! 
12. Filter sehingga wireshark hanya mengambil paket yang mengandung port 21!
13. Filter sehingga wireshark hanya menampilkan paket yang menuju port 443!
14. Filter sehingga wireshark hanya mengambil paket yang tujuannya ke kemenag.go.id!
15. Filter sehingga wireshark hanya mengambil paket yang berasal dari ip kalian!

## Jawaban:
**Soal 1**
> Sebutkan web server yang digunakan pada "ichimarumaru.tech"! 

Jawab:

Langkah-langkah yang dilakukan untuk menemukan web server “ichimarumaru.tech” adalah:
Wireshark Display filter: `http.host == “ichimarumaru.tech”`
<img alt="" src="images/image5.png">
Klik kanan pada salah satu package, kemudian klik Follow, lalu pilih TCP stream. Atau bisa menggunakan Wireshark Display Filter: `tcp.stream eq 12`. Hasilnya adalah sebagai berikut
<img alt="" src="images/image13.png">

Didapatkan web server yang digunakan adalah `nginx/1.18.0 (Ubuntu)`.

**Soal 2**
>Temukan paket dari web-web yang menggunakan basic authentication method!

Jawab:

Wireshark Display Filter yang digunakan: `http.authbasic`. Hasilnya:
<img alt="" src="images/image4.png">

**Soal 3**

Ikuti perintah di basic.ichimarumaru.tech! Username dan password bisa didapatkan dari file .pcapng!
Jawab:
Untuk menemukan username dan password:
IP address dari ichimarumaru.tech adalah 167.172.77.139 (dapat dilihat pada bagian sebelumnya). Kemudian menggunakan filter request untuk melihat data package yang merequest GET / POST yang berisi data login / authorization.
Wireshark Display Filter : `ip.dst == 167.172.77.139 && http.request`. Hasilnya:
<img alt="" src="images/image2.png">

Dari macam-macam package tersebut, dicari salah satu package yang menyimpan data Authorization. Pada kasus ini ditemukan pada package no. 568. Ditemukan username:kuncimenujulautan dan passwordnya:tQKEJFbgNGC1NCZlWAOjhyCOm6o3xEbPkJhTciZN
<img alt="" src="images/image6.png">

Hasil jawaban pada basic.ichimaru.tech
<img alt="" src="images/image19.png">

**Soal 4**
>Temukan paket mysql yang mengandung perintah query select!

Jawab:

Wireshark Display Filter : `tcp matches “select”` . Hasilnya:
<img alt="" src="images/image28.png">

**Soal 5**
> Login ke portal.ichimarumaru.tech kemudian ikuti perintahnya! Username dan password bisa didapat dari query insert pada table users dari file .pcap!

Jawab:

Untuk mendapatkan username dan password, menggunakan Wireshark Display Filter: `tcp matches “insert”` . Hasilnya didapatkan username: akakanomi dan password: pemisah4lautan.
<img alt="" src="images/image24.png">

Hasil pengerjaan di http://portal.ichimarumaru.tech/ :
<img alt="" src="images/image29.png">

**Soal 6**
> Cari username dan password ketika melakukan login ke FTP Server!

Jawab: 

Wireshark filter expression: 
ftp.request.command==USER || ftp.request.command==PASS
<img alt="" src="images/image15.png">

Username: secretuser
100	7.304845	::1	::1	FTP	81	Request: USER secretuser
Password: aku.pengen.pw.aja
104	7.305064	::1	::1	FTP	88	Request: PASS aku.pengen.pw.aja

**Soal 7**
> Ada 500 file zip yang disimpan ke FTP Server dengan nama 0.zip, 1.zip, 2.zip, ..., 499.zip. Simpan dan Buka file pdf tersebut. (Hint = nama pdf-nya "Real.pdf")

Jawab:

Wireshark filter expression:
`tcp contains “Real.pdf” ` atau `frame contains “Real.pdf”`
<img alt="" src="images/image26.png">

Follow TCP stream, show data as Raw, save as [nama].zip
<img alt="" src="images/image10.png">

Hasil unzip file Real.pdf:
<img alt="" src="images/image1.png">


**Soal 8**
> Cari paket yang menunjukan pengambilan file dari FTP tersebut!

Jawab:
Filter `ftp-data.command contains "RETR"`
<img alt="" src="images/image12.png">

Hasilnya adalah tidak ada paket.

**Soal 9**
> Dari paket-paket yang menuju FTP terdapat indikasi penyimpanan beberapa file. Salah satunya adalah sebuah file berisi data rahasia dengan nama "secret.zip". Simpan dan buka file tersebut!

Jawab:

Filter `ftp-data.command contains "secret.zip"`
<img alt="" src="images/image9.png">

Klik kanan package ftp-data yang memiliki jumlah bytes paling beda (1002 bytes). Follow > TCP Stream. Show data as “Raw” dan save dalam nama “secret.zip”.
<img alt="" src="images/image11.png">

Save. Serta buka file “secret.zip”.
<img alt="" src="images/image27.png">

**Soal 10**
> Selain itu terdapat "history.txt" yang kemungkinan berisi history bash server tersebut! Gunakan isi dari "history.txt" untuk menemukan password untuk membuka file rahasia yang ada di "secret.zip"!

Jawab:

Filter `ftp-data.command contains “history.txt”`
<img alt="" src="images/image22.png">

Klik kanan satu-satunya package ftp-data. Follow > TCP Stream. Show data as “ASCII”.
<img alt="" src="images/image25.png">

Ditemukan kode bash sebagai berikut.
```
 288  ls
  289  key="$(tail -1 bukanapaapa.txt)"
  290  zip -P $key secret.zip Wanted.pdf
  291  rm Wanted.pdf
  292  history | tail -5 > history.txt
```
Karena memerlukan mendownload bukanapaapa.txt, maka lakukan filter dengan `ftp-data.command contains "bukanapaapa.txt"`
<img alt="" src="images/image20.png">

Klik kanan satu-satunya package ftp-data. Follow > TCP Stream. Show data as “ASCII”.
<img alt="" src="images/image23.png">

Save dengan nama “bukanapaapa.txt”.
Lalu, jalankan WSL dan masukkan bash `tail -1 bukanapaapa.txt`, maka didapat password “d1b1langbukanapaapajugagapercaya”. Masukkan password pada zip didapat:
<img alt="" src="images/image21.png">

Hasil
<img alt="" src="images/image18.png">

**Soal 11**
> Filter sehingga wireshark hanya mengambil paket yang berasal dari port 80! 

Jawab:

Gunakan Wireshark filter expression (capture): src port 80
<img alt="" src="images/image3.png">

**Soal 12**
> Filter sehingga wireshark hanya mengambil paket yang mengandung port 21!
Jawab:
Gunakan Wireshark filter expression (capture): port 21
Kosong karena tidak ada FTP yang sedang berlangsung
<img alt="" src="images/image8.png">

**Soal 13**
> Filter sehingga wireshark hanya menampilkan paket yang menuju port 443!
Jawab:
Gunakan Wireshark filter expression (display): tcp.dstport == 443
<img alt="" src="images/image7.png">

**Soal 14** 
> Filter sehingga wireshark hanya mengambil paket yang tujuannya ke kemenag.go.id!
Jawab:
IP address. kemenag.go.id adalah 103.7.13.247.
Gunakan Wireshark filter expression (capture): dst kemenag.go.id
<img alt="" src="images/image17.png">

**Soal 15**
> Filter sehingga wireshark hanya mengambil paket yang berasal dari ip kalian!
Jawab:
Gunakan ipconfig untuk mengetahui IP address device (192.168.1.4)
<img alt="" src="images/image14.png">

Gunakan Wireshark filter expression (capture):
ip src 192.168.1.4
<img alt="" src="images/image16.png">

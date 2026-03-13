Implementasi prosedur recovery data hingga titik waktu spesifik (PITR) dengan mengombinasikan physical full backup (menggunakan percona xtraBackup) dan binary log. Bertujuan untuk meminimalkan data loss akibat human error atau kegagalan sistem

fungsi binary log:
- file binary log mencatat semua modifikasi data logis yang terjadi
- berjalan di atas database mysql server
- berfungsi untuk replikasi dan pitr



1. Melakukan modifikasi pada mysqld.cnf untuk mengaktifkan log binari sebagai pencatat semua modifikasi/perubahan data logis di server
   
   <img width="831" height="145" alt="Screenshot (217)" src="https://github.com/user-attachments/assets/d6d54211-1e3a-4ecd-b46b-6af7a6ad3337" />
   

2. Melakukan audit runtime. Memastikan binary log telah aktif dan siap recording transaksi

   <img width="977" height="410" alt="Screenshot (250)" src="https://github.com/user-attachments/assets/37b53e57-62ef-4885-9381-bf3f06335a84" />


3. Membuat sampel data sebagai testing titik referensi recovery data (PITR) sebelum dilakukan full backup
   
   <img width="1685" height="791" alt="Screenshot (267)" src="https://github.com/user-attachments/assets/d21f2920-f47e-47db-a8c6-d19793adc94a" />


4. Membuat direktori backup sebagai tempat storage backup sementara
   
   <img width="934" height="84" alt="Screenshot (268)" src="https://github.com/user-attachments/assets/90c5863e-6a17-442c-ab78-3bd32a41e16d" />


5. Eksekusi physical full backup dengan xtraBackup untuk mengambil salinan fisik database secara hot backup (tanpa mengunci database)
   
   <img width="1697" height="274" alt="Screenshot (269)" src="https://github.com/user-attachments/assets/69e4a05f-ff51-4683-8afc-29328133d3ac" />


6. Validasi direktori backup. Memastikan semua komponen data termasuk binary log sudah tersalin
   
   <img width="1291" height="472" alt="Screenshot (274)" src="https://github.com/user-attachments/assets/1e6e15ad-dbef-4abe-89bf-d7a61f9dc9b8" />


7. Menjalankan fase --prepare pada backupset untuk menjamin konsistensi transaksional (aplikasi redo log) sebelum data dikirim ke target server
   
   <img width="1685" height="267" alt="Screenshot (270)" src="https://github.com/user-attachments/assets/1d192c3b-2054-4d6b-95d5-6c7adeecce8c" />


8. Migrasi file backup dari source server ke server target menggunakan protokol Rsync. Menjamin kecepatan dan keamanan transfer file
   
   <img width="1306" height="252" alt="Screenshot (272)" src="https://github.com/user-attachments/assets/7fca4185-8625-47e6-aa4a-64bd8a52064a" />


9. Validasi salinan data pada server target. Memastikan salinan compatable dengan source server
   
   <img width="1348" height="442" alt="Screenshot (273)" src="https://github.com/user-attachments/assets/942e6216-bc3f-47a7-a79e-cafca99a7a99" />


10. Add new data pada resource database setelah restorasi selesai. Data ini terekam di binary log dan menjadi target pemulihan PITR pada target server
    
    <img width="1085" height="830" alt="Screenshot (275)" src="https://github.com/user-attachments/assets/8383d451-b446-4a34-9b01-5225c890b64d" />


11. Melakukan inspeksi file log binari di /var/log/mysql pada source server dan transfer binlog ke target server sebagai bahan recovery data terbaru
    
    <img width="1158" height="236" alt="Screenshot (276)" src="https://github.com/user-attachments/assets/1b910af5-2f06-4870-92c8-41a2183b6770" />
    <img width="1667" height="147" alt="Screenshot (277)" src="https://github.com/user-attachments/assets/9767e44c-13e2-4daf-9de8-26dd342e950c" />


12. Validasi existensi new binlog pada direktori backup storage di target server
    
    <img width="1450" height="163" alt="Screenshot (278)" src="https://github.com/user-attachments/assets/a9a4bfa3-db99-431f-8262-51f445f4b4ad" />


13. Memindahkan new binlog ke folder /var/log/mysql pada target server. Memastikan database target dapat membaca eksistensi binary log untuk recovery new data
    
    <img width="1330" height="530" alt="Screenshot (280)" src="https://github.com/user-attachments/assets/0401db54-5740-47bb-83fc-23355f4fa4bd" />


14. Melakukan stop service MySQL dan cleanup direktori /var/lib/mysql pada target server untuk menghindari konflik saat restorasi fisik
    
    <img width="1350" height="365" alt="Screenshot (279)" src="https://github.com/user-attachments/assets/38cfe13c-dc7e-4392-b072-ff14dabb3d4d" />
    <img width="1694" height="327" alt="Screenshot (282)" src="https://github.com/user-attachments/assets/c006ef66-ce38-4fab-b290-f814dce2da83" />


15. Melakukan perubahan ownership /var/lib/mysql dan /var/log/mysql ke user mysql dan menaktifkan service MySQL
    - di sini data masih menyimpan data lama saat pertama kali membuat full backup di server primary
    - untuk mengambil data baru yang sudah diinput ke dalam table, perlu restore binary log terakhir
      
      <img width="1012" height="776" alt="Screenshot (283)" src="https://github.com/user-attachments/assets/9de97a22-2830-4b14-9cc5-5a69e7e14c62" />


16. Menjalankan utilitas mysqlbinlog untuk mengaplikasikan transaksi terbaru dari file log binari ke dalam database yang baru direstore dan validasi data integrity 
    
    <img width="1231" height="726" alt="Screenshot (284)" src="https://github.com/user-attachments/assets/588c6255-28f5-4f87-9a23-1f1cfa49203d" />





##selanjutnya saya akan mencoba migrasi data dari server primary ke server standby menggunakan metode replikasi master-slave
   
   

Demonstrasi proses query optimization & database maintenance untuk menjaga integritas struktural, akurasi metadata statistik, dan efisiensi execution plan. Bertujuan mereduksi penggunaan sumber daya (resource consumption) dan meningkatkan throughput queri yang memiliki workload tinggi


1. Melakukan verifikasi integritas fisik pada struktur tabel. Mendeteksi data corrupt atau inkonsistensi pada struktur index

   <img width="1215" height="395" alt="Screenshot (318)" src="https://github.com/user-attachments/assets/4589bd5f-4d67-45d6-9318-78650b8aee6e" />


2. Melakukan update statistik metadata agar cost-based optimizer (CBO) dapat menghasilkan access path paling efisien

   <img width="1094" height="267" alt="Screenshot (319)" src="https://github.com/user-attachments/assets/2d4df3d6-994e-4283-8686-981f6e847e90" />
   <img width="954" height="343" alt="Screenshot (321)" src="https://github.com/user-attachments/assets/c74c593e-6570-4515-8e12-caa4a712cfcf" />
   - Cardinality : Semakin akurat, semakin tepat keputusan optimizer dalam menentukan metode join atau penggunaan index
   - Visible     : Memastikan index berada dalam status visible agar dapat dipertimbangkan oleh optimizer saat proses queri


3. Melakukan inspeksi terhadap eecution plan untuk mengidentifikasi inefisiensi seperti full table scan.

   <img width="1309" height="569" alt="Screenshot (326)" src="https://github.com/user-attachments/assets/353f35f9-845a-4dce-a214-e08a2b2cd277" />
   - Actual-time : 0.0332
   - Scan-rows   : queri scan 10 baris (scan-rows) untuk mendapatkan 5 baris data, yang menunjukkan inefisiensi pada tabel skala besar


4. Akselerasi query optimizer melalui strategi physical tuning (indexing)

   <img width="1648" height="403" alt="Screenshot (327)" src="https://github.com/user-attachments/assets/ce41035e-1b1f-4d8c-8a3a-58c2323bde0e" />
   - Actual-time : 0.0332 > 0.0276    
   - Scan-rows   : 10 > 5, scan-rows menjadi 5 baris membuktikan efektivitas index dalam meminimalkan I/O

  
5. Melakukan audit semua existing index untuk mengidentifikasi struktur yang redundan (duplicate), guna mengurangi overhead costs pada operasi DML

   <img width="1167" height="172" alt="Screenshot (328)" src="https://github.com/user-attachments/assets/998bcdf0-a773-4fd5-bc15-4fb4551f9af1" />


6. Monitoring index bloat akibat aktivitas DML yang intensif

   <img width="1270" height="429" alt="Screenshot (329)" src="https://github.com/user-attachments/assets/76d23c87-a629-4615-8d42-f5d387317044" />
   - Free_mb      : merupakan empty space yang terfragmentasi. apabila tinggi, table tersebut terfagmentasi
   - Frag_percent : seberapa besar tabel terfragmentasi oleh operasi DML
   - Index_mb     : memori index dalam menginisiasi penggunaan index


7. Melakukan monitoring terhadap utilitas index yang digunakan
   
   <img width="1270" height="395" alt="Screenshot (330)" src="https://github.com/user-attachments/assets/55447a4a-fe38-4588-9423-970255e7ec12" />
   - Total_acess   : seberapa sering index digunakan/diakses




   
   
   

   
   


   

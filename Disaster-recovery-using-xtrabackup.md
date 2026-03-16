Implementing recovery procedur with Point in Time Reocvery (PITR) combine binary log using xtrabackup to return data before data loss due to human error or system failure 

binary log funxtion:
- Binary log file record all modify data logic happens
- To function for disaster recovery 


1. Do modify on mysqld.conf to enable binary log as recording all modify data logic/changer data on database instance
   
   <img width="831" height="145" alt="Screenshot (217)" src="https://github.com/user-attachments/assets/d6d54211-1e3a-4ecd-b46b-6af7a6ad3337" />
   

2. Verify audit to ensure binary log has enable on database instance with query statement 

   <img width="977" height="410" alt="Screenshot (250)" src="https://github.com/user-attachments/assets/37b53e57-62ef-4885-9381-bf3f06335a84" />


3. Create data sample as testing disaster recovery with PITR
   
   <img width="1685" height="791" alt="Screenshot (267)" src="https://github.com/user-attachments/assets/d21f2920-f47e-47db-a8c6-d19793adc94a" />


4. Create directory backup as temporary location storage backup on OS level
   
   <img width="934" height="84" alt="Screenshot (268)" src="https://github.com/user-attachments/assets/90c5863e-6a17-442c-ab78-3bd32a41e16d" />


5. Execute physical backup with xtrabackup to take all copy physical datafile with binary log without lock table
   
   <img width="1697" height="274" alt="Screenshot (269)" src="https://github.com/user-attachments/assets/69e4a05f-ff51-4683-8afc-29328133d3ac" />


6. Validate directory backup to ensure existing metada backup with binary log
   
   <img width="1291" height="472" alt="Screenshot (274)" src="https://github.com/user-attachments/assets/1e6e15ad-dbef-4abe-89bf-d7a61f9dc9b8" />


7. Run xtrabackup with fitur --prepare to take transaction log to binary log 
   
   <img width="1685" height="267" alt="Screenshot (270)" src="https://github.com/user-attachments/assets/1d192c3b-2054-4d6b-95d5-6c7adeecce8c" />


8. Migrate file backup from source server to target server using safe protocol RSYNC to ensure Faster migrate file and security transfer file

   <img width="1306" height="252" alt="Screenshot (272)" src="https://github.com/user-attachments/assets/7fca4185-8625-47e6-aa4a-64bd8a52064a" />


9. Validate backup copy on target server and verify compatible with source server
   
   <img width="1348" height="442" alt="Screenshot (273)" src="https://github.com/user-attachments/assets/942e6216-bc3f-47a7-a79e-cafca99a7a99" />


10. Modify data on source database and database instance will be safe data modify to binary log
    
    <img width="1085" height="830" alt="Screenshot (275)" src="https://github.com/user-attachments/assets/8383d451-b446-4a34-9b01-5225c890b64d" />


11. Do file inspection binary log on /var/log/mysql on source server and transfer new binary log after data modify as recovery data with PITR
    
    <img width="1158" height="236" alt="Screenshot (276)" src="https://github.com/user-attachments/assets/1b910af5-2f06-4870-92c8-41a2183b6770" />
    <img width="1667" height="147" alt="Screenshot (277)" src="https://github.com/user-attachments/assets/9767e44c-13e2-4daf-9de8-26dd342e950c" />


12. Validate existing new binary log on directory backup store on targer server
    
    <img width="1450" height="163" alt="Screenshot (278)" src="https://github.com/user-attachments/assets/a9a4bfa3-db99-431f-8262-51f445f4b4ad" />


13. Move new binary log to directory /var/log/mysql on target server to ensure database instance can be reading binary log to changer data on database instance 
    
    <img width="1330" height="530" alt="Screenshot (280)" src="https://github.com/user-attachments/assets/0401db54-5740-47bb-83fc-23355f4fa4bd" />


14. Do stop service database MySQL and cleanup directory /var/lib/mysql on target server to avoid conflict physical restore
    
    <img width="1350" height="365" alt="Screenshot (279)" src="https://github.com/user-attachments/assets/38cfe13c-dc7e-4392-b072-ff14dabb3d4d" />
    <img width="1694" height="327" alt="Screenshot (282)" src="https://github.com/user-attachments/assets/c006ef66-ce38-4fab-b290-f814dce2da83" />


15. Change ownership /var/lib/mysql and /var/log/mysql to user MySQL for database instance can be reading data directory and binary log for running database           instance
      
      <img width="1012" height="776" alt="Screenshot (283)" src="https://github.com/user-attachments/assets/9de97a22-2830-4b14-9cc5-5a69e7e14c62" />


16. Restore new binary log with mysqlbinlog utility to changer data on database instance  
    
    <img width="1231" height="726" alt="Screenshot (284)" src="https://github.com/user-attachments/assets/588c6255-28f5-4f87-9a23-1f1cfa49203d" />





##selanjutnya saya akan mencoba migrasi data dari server primary ke server standby menggunakan metode replikasi master-slave
   
   

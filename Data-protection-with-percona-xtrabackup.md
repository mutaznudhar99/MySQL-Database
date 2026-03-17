I implemented a physical database migration to a new MySQL server using Percona XtraBackup. As an open-source tool, XtraBackup is an excellent alternative to MySQL Enterprise Backup.

Advantages of xtrabackup:
- Open Source: Free to use, providing enterprise-level features for MySQL.
- Hot Backup: It performs non-blocking backups for InnoDB, so the database stays online during the process.
- Data Integrity: It ensures data consistency using the --prepare feature to apply transaction logs to the backup files.
- Complete Features: Supports compression, parallel processing, incremental backups, and fast recovery.


1. Created a testing schema and tables to validate data integrity after the restore process.

   - Verified existing schema
     <img width="803" height="218" alt="Screenshot (168)" src="https://github.com/user-attachments/assets/c7976a42-d017-488d-9965-2c887ee4c5ca" />


   - Created new schema with data table structure
     <img width="1565" height="429" alt="Screenshot (173)" src="https://github.com/user-attachments/assets/14a6d927-d252-458c-9f87-06ba719dbdb0" />
     <img width="694" height="233" alt="Screenshot (175)" src="https://github.com/user-attachments/assets/abde9a4a-82b5-42c9-a468-7fb696a8f936" />


   - Verified existing schema to ensure new schem has adding on database instance
     <img width="948" height="235" alt="Screenshot (176)" src="https://github.com/user-attachments/assets/8b50d5bf-cd91-4bd5-a089-2bfb4a75932b" />
     <img width="907" height="147" alt="Screenshot (174)" src="https://github.com/user-attachments/assets/57b24600-fb6b-4c46-90c2-6452260e6e79" />

   - Verified size schema before take backup
     <img width="1113" height="576" alt="Screenshot (194)" src="https://github.com/user-attachments/assets/53de68a1-6ce4-497f-8c36-294fca5f8412" />



2. Created a temporary storage location for the backup files in the /mnt directory.
   
   <img width="942" height="223" alt="Screenshot (178)" src="https://github.com/user-attachments/assets/34c1a221-ffde-400f-8e88-ac004ef18a49" />



3. Verified the size of /var/lib/mysql using du -h to ensure the target server has enough disk space.

   <img width="733" height="184" alt="Screenshot (177)" src="https://github.com/user-attachments/assets/6dc519da-4404-422a-b7e8-3c560db9bac5" />



4. Ran the backup command with compression: xtrabackup --backup --compress
     
   <img width="1692" height="382" alt="Screenshot (179)" src="https://github.com/user-attachments/assets/40c718b4-a702-432b-951a-a4aec3839d41" />



5. Move the backup data to the target server Used secure protocols (RSYNC/SCP)
     
   <img width="1001" height="221" alt="Screenshot (209)" src="https://github.com/user-attachments/assets/58ed636b-8d51-4528-a150-7e173e9389b3" />


6. Updated the directory ownership to the mysql user so the MySQL instance can read the files backup
   
   <img width="1150" height="424" alt="Screenshot (203)" src="https://github.com/user-attachments/assets/af804b47-8db5-4409-b84b-93b8ed9d5c45" />


7. Executed the --prepare command to apply the redo logs, ensuring the data is consistent

   <img width="1687" height="334" alt="Screenshot (191)" src="https://github.com/user-attachments/assets/a22f692c-1277-435d-b630-1181169ed054" />


8. Deleted the existing /var/lib/mysql directory on the target server to prepare for the new data. 

   <img width="797" height="69" alt="Screenshot (204)" src="https://github.com/user-attachments/assets/57833c3a-8874-4860-a6b5-23358edad331" />


9. Moved the prepared backup files into the MySQL data directory with restore database
    
    <img width="1688" height="394" alt="Screenshot (205)" src="https://github.com/user-attachments/assets/56674a4c-5ac8-413c-a631-3debc4b3d214" />


10. Started the MySQL service and ran query statements to verify that all data was restored successfully with zero data loss.
    
    <img width="886" height="496" alt="Screenshot (207)" src="https://github.com/user-attachments/assets/07a1c11f-690e-4595-bafc-50ead031e412" />
    <img width="1068" height="325" alt="Screenshot (208)" src="https://github.com/user-attachments/assets/435700fc-8c7a-49bd-bf87-7a6bd6c800f0" />
    <img width="1157" height="207" alt="Screenshot (215)" src="https://github.com/user-attachments/assets/d8abfedd-9f41-4607-bdbc-ab730be53ef7" />




   

   


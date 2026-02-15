Oracle Multitenant Architecture – Oracle 21c Pluggable Database (PDB) Administration
Overview
This assignment demonstrates practical skills in Oracle Multitenant Architecture including creation, management, and deletion of Pluggable Databases (PDBs), user management inside PDBs, and monitoring using Oracle Enterprise Manager (OEM).

Task 1 Creation of a pluggable Database
Before creating a Pluggable Database you first check Current container and then find the Current data location which will be used in the creation of the PDB.

To check the Current container use:

sqlplus / as sysdba
SHOW CON_NAME;
This is advised to be done everytime because it assures the user that he/she is in the correct container.

Then for finding the Current data location use:

SELECT name 
FROM v$datafile 
WHERE con_id = 2;
 
<img width="1536" height="1024" alt="Capture1" src="https://github.com/user-attachments/assets/bebc6168-d71e-456c-8944-632b8b70ca98" />

The next step is to create a PDB:

CREATE PLUGGABLE DATABASE ng_pdb_27980
ADMIN USER pdbadmin IDENTIFIED BY 27980
FILE_NAME_CONVERT = ('C:\USERS\NGOGA\DOWNLOADS\ORADATA\ORCL\PDBSEED\','C:\USERS\NGOGA\DOWNLOADS\ORADATA\ORCL\PDBSEED\ng_pdb_27980\');

CREATE PLUGGABLE DATABASE : creates the new PDB
ADMIN USER pdbadmin : creates an administrative user for that PDB
FILE_NAME_CONVERT : tells Oracle where to copy the datafiles from the seed database. The first path before the comma is the Old path which is the Current data location we found in the above steps and the next path is the New path which has same name as our created PDB.
<img width="1536" height="1024" alt="Capture2" src="https://github.com/user-attachments/assets/01239772-3a5d-4060-828a-1cdb8e8a5813" />


After creating the PDB OPEN it and verify that the OPEN MODE should now show READ WRITE.

ALTER PLUGGABLE DATABASE ng_pdb_27980 OPEN;
SHOW PDBS;
<img width="1536" height="1024" alt="Capture3" src="https://github.com/user-attachments/assets/f559d175-87f1-4561-913a-2bc27a613527" />


The next step is about creating a new user in our PDB.

Firstly, switch your session to the created PDB:

ALTER SESSION SET CONTAINER = ng_pdb_27980;
Verify if the container has changed using

SHOW CON_NAME; 
<img width="1536" height="1024" alt="Capture4" src="https://github.com/user-attachments/assets/be7b2d7b-ac87-4c2c-99c9-6997953eb944" />

After running this you must see the name of your newly created PDB. Then create the new user and grant privileges.

CREATE USER user_name
IDENTIFIED BY password;
<img width="1536" height="1024" alt="Capture5" src="https://github.com/user-attachments/assets/f56c4b75-681e-4d28-8ac4-7b07931ab88a" />


After creating a user you can grant privileges (this depends on the requirements of a given project) by running:

GRANT ALL PRIVILEGES TO username;
To verify if the user is created use:

SELECT username FROM dba_users
WHERE username = 'user_name'
<img width="1536" height="1024" alt="Capture6" src="https://github.com/user-attachments/assets/f08378a4-7705-4a13-a75c-724d084da795" />



Task 2: Creating and deleting a temporary PDB
Firstly, we must switch back to ROOT:

ALTER SESSION SET CONTAINER = CDB$ROOT;
SHOW CON_NAME;

<img width="716" height="136" alt="Capture7" src="https://github.com/user-attachments/assets/2753b016-fbbb-4f65-8f8b-fd9bb0016a09" />


Secondly, create the Temporary PDB using:

CREATE PLUGGABLE DATABASE ng_to_delete_pdb_27980
ADMIN USER pdbadmin IDENTIFIED BY 27980
FILE_NAME_CONVERT = ('C:\USERS\NGOGA\DOWNLOADS\ORADATA\ORCL\PDBSEED\','C:\USERS\NGOGA\DOWNLOADS\ORADATA\ORCL\PDBSEED\ng_to_delete_pdb_27980\');

Then check if it exists using:
<img width="1536" height="1024" alt="Capture8" src="https://github.com/user-attachments/assets/98c6a4ad-c81f-43de-9481-975dbf7d1bd3" />



To delete the PDB we just created, run:

DROP PLUGGABLE DATABASE ng_to_delete_pdb_27980 INCLUDING DATAFILES;
Then we check if the deletion was successful using:

SHOW PDBS;
<img width="1536" height="1024" alt="Capture9" src="https://github.com/user-attachments/assets/f99433a3-98f0-4826-9656-277b70cc712c" />

Task 3: Oracle Entreprise Manager (OEM)
Firstly, it is highly advised to create a connection from our Oracle PDB user to SQL developer.

<img width="1536" height="1024" alt="Capture10" src="https://github.com/user-attachments/assets/c64165c2-8a2a-4c1b-bdfb-de3e970654da" />






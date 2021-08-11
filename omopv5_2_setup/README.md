## How to install PostgreSQL database for OMOPv5.2 using Docker
1. Goto Athena.ohdsi.org to download Vocabularies. The downloaded
   zip file has some additional instruction for CPT code. Please follow it
	 to get vocabulary dataset.
2. Get docker image of PostgreSQL: docker pull postgres
3. Run docker run --name omopv60 -d -p 5432:5432 -e POSTGRES_PASSWORD=<password> --restart unless-stopped postgres:latest
4. Create database and schema
create database <databasename> with owner postgres;
5. Run DDLs from CommonDataModel folder. Go to the folder for PostgreSQL.
psql -h localhost -p <port> -U postgres -W -d <database> -f CommonDataModel/OMOP\ CDM\ postgresql\ ddl.txt
5.1 You may want to run the follow if you want to index them.
  psql -h localhost -p <port> -U postgres -W -d <database> -f CommonDataModel/PostgreSQL/OMOP\ CDM\ indexes\ required\ -\ PostgreSQL.sql
6. Run DDLs for f_observation_view, and f_person
  omoponfhir_f_person_ddl.txt
  omoponfhir_v5.2_f_observation_view_ddl.txt
7. Load vocabularies downloaded from Athena.
7.1 Change the ddl in CommonDataModel/PostgreSQL/VocaImport/ to point to the vocabulary files. And change COPY to \copy if you are running it from OSX.
8. (Optional only if you have your own data loaded to OMOP database and f_person table is empty)
   Use fhir_names/ folder to load f_person. Read the instruction in fhir_names/ folder.
	 This will create entries with random names for the entries in the person table.
9. Index the tables using CommonDataModel/PostgreSQL/OMOP\ CDM\ indexes\ required\ -\ PostgreSQL.sql

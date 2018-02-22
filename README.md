# Hive Data Definition and Manipulation Tutorial
1. Downlaod the dataset to your VM
```
wget https://site.ada.edu.az/~aadamov/sources/data/Salaries.csv
```
2. To use Hive the interpretor should start typing hive
 
Default location of Hive data is `/user/hive/warehouse` (Hive's warehouse directory)
To run system commands from Hive put "!" as prefix of command. Example: `hive>!ls /home/hadoop` or `/ hive>!hdfs dfs -ls /`

3. To see the list of available databases
```
SHOW DATABASES;
```
4. To create new database
```
CREATE DATABASE mydb;
```
as a result of this command new directory "mydb.db" will be created within Hive default folder
```
hdfs dfs -ls /user/hive/warehouse/
```
5. To activate specific databse
```
USE mydb;
```
6. To create new table associated with active database
```
CREATE TABLE salaries (id STRING,
rank STRING,
discipline STRING,
yrsphd INT, 
yrsservice INT,
sex STRING,
salary DOUBLE) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' STORED AS TEXTFILE 
TBLPROPERTIES("skip.header.line.count"="1");
```
as a result of this command appropriate metadata added to Metastore and new directory "salaries" will be created within database directory "mydb.db". Some important properties of the new table are defined: columns are delimited by comma. File is stored as a textual file (Hive supports following four file-types to store data TEXTFILE, SEQUENCEFILE, ORC and RCFILE). Specific property of table set to skip first row of data-file which consists of column names.
```
hdfs dfs -ls /user/hive/warehouse/mydb.db
```
7. To verify the structure of table
```
DESCRIBE salaries;
```
7. To load data from "Salaries.csv" file that located in local file system into newly created "salaries" table
```
LOAD DATA LOCAL INPATH '/home/hadoop/Salaries.csv' OVERWRITE INTO TABLE salaries;
```
Now look into directory "salaries" associated with table and see "Salaries.csv". Try to understand what does it mean and how it follow to Schema on READ (instead on WRITE)
```
hdfs dfs -ls /user/hive/warehouse/mydb.db/salaries
```
8. Now to retries a data from Hive table "salaries"
```
SELECT * FROM salaries LIMIT 10;
```
9. To initiate MapReduce job associated with Hive query 
```
SELECT COUNT(*) FROM salaries;
```

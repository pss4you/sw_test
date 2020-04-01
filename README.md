# TPC-H test data generation

How to Use(For Oracle case)

-- All of the following should be done from the DBMS server terminal.
1. create DBMS user account
2. execute dbgen file with option to tpch data creation
3. open the .tbl files and paste the paragraphs at top of each file 
4. create tpc-h tables in DBMS
5. load data into DBMS using SQLLoader
6. Perform SW test using the loaded data
7. If you want to use tpc-h queries, create queries using 'qgen'

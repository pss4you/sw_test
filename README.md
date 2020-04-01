# TPC-H test data generation

How to Use(For Oracle case)

-- All of the following should be done from the DBMS server terminal.

1. create DBMS user account
e.g.)
1. sqlplus "/as sysdba"
2. create user testuser identified by testuser;
3. grant dba to testuser;
4. exit;
 
2. execute dbgen file with option to tpch data creation
e.g.)
 ./dbgen -s 10
 note about dbgen
  - '-s' option means 'scale factor'
  - 1 scale factor data size = 1GB
  - 10 scale factor data size = 10GB

3. open the .tbl files and paste the paragraphs at top of each file 

--- supplier.tbl ----
LOAD DATA
INFILE *
TRUNCATE
INTO TABLE "SUPPLIER"
FIELDS TERMINATED BY '|'
TRAILING NULLCOLS (
S_SUPPKEY,
S_NAME,
S_ADDRESS,
S_NATIONKEY,
S_PHONE,
S_ACCTBAL,
S_COMMENT
)
begindata

--- region.tbl ----
LOAD DATA
INFILE *
TRUNCATE
INTO TABLE "REGION"
FIELDS TERMINATED BY '|'
TRAILING NULLCOLS (
R_REGIONKEY,
R_NAME,
R_COMMENT
)
begindata

--- partsupp.tbl ----
LOAD DATA
INFILE *
TRUNCATE
INTO TABLE "PARTSUPP"
FIELDS TERMINATED BY '|'
TRAILING NULLCOLS (
PS_PARTKEY,
PS_SUPPKEY,
PS_AVAILQTY,
PS_SUPPLYCOST,
PS_COMMENT
)
begindata

--- part.tbl ----
LOAD DATA
INFILE *
TRUNCATE
INTO TABLE "PART"
FIELDS TERMINATED BY '|'
TRAILING NULLCOLS (
P_PARTKEY,
P_NAME,
P_MFGR,
P_BRAND,
P_TYPE,
P_SIZE,
P_CONTAINER,
P_RETAILPRICE,
P_COMMENT
)
begindata

--- orders.tbl ----
LOAD DATA
INFILE *
TRUNCATE
INTO TABLE "ORDERS"
FIELDS TERMINATED BY '|'
TRAILING NULLCOLS (
O_ORDERKEY,
O_CUSTKEY,
O_ORDERSTATUS,
O_TOTALPRICE,
O_ORDERDATE DATE "yyyy-mm-dd",
O_ORDERPRIORITY, 
O_CLERK,
O_SHIPPRIORITY,
O_COMMENT
)
begindata

--- nation.tbl ----
LOAD DATA
INFILE *
TRUNCATE
INTO TABLE "NATION"
FIELDS TERMINATED BY '|'
TRAILING NULLCOLS (
N_NATIONKEY,
N_NAME,
N_REGIONKEY,
N_COMMENT
)
begindata

--- lineitem.tbl ----
LOAD DATA
INFILE *
TRUNCATE
INTO TABLE "LINEITEM"
FIELDS TERMINATED BY '|'
TRAILING NULLCOLS (
L_ORDERKEY,
L_PARTKEY,
L_SUPPKEY,
L_LINENUMBER,
L_QUANTITY,
L_EXTENDEDPRICE,
L_DISCOUNT,
L_TAX,
L_RETURNFLAG,
L_LINESTATUS,
L_SHIPDATE DATE "yyyy-mm-dd",
L_COMMITDATE DATE "yyyy-mm-dd",
L_RECEIPTDATE DATE "yyyy-mm-dd",
L_SHIPINSTRUCT,
L_SHIPMODE,
L_COMMENT
)
begindata

--- customer.tbl ----
LOAD DATA
INFILE *
TRUNCATE
INTO TABLE "CUSTOMER"
FIELDS TERMINATED BY '|'
TRAILING NULLCOLS (
C_CUSTKEY,
C_NAME,
C_ADDRESS,
C_NATIONKEY,
C_PHONE,
C_ACCTBAL,
C_MKTSEGMENT,
C_COMMENT
)
begindata

4. create tpc-h tables in DBMS
e.g.)
 sqlplus -S testuser/testuser < dss.ddl

5. load data into DBMS using SQLLoader

 sqlldr testuser/testuser control=customer.tbl
 sqlldr testuser/testuser control=lineitem.tbl
 sqlldr testuser/testuser control=nation.tbl
 sqlldr testuser/testuser control=orders.tbl
 sqlldr testuser/testuser control=part.tbl
 sqlldr testuser/testuser control=partsupp.tbl
 sqlldr testuser/testuser control=region.tbl
 sqlldr testuser/testuser control=supplier.tbl

 note 
  - Make sure that the free hard disk space of the DBMS server is at least 15GB

6. Perform SW test using the loaded data

7. If you want to use tpc-h queries, create queries using 'qgen'
e.g.)
  ./qgen -s 10 9
  note about qgen
  - '-s' option means 'scale factor'
  - The '-s 10' option means to create a query that fits the scale factor 10 data.
  - The last number means the query you want to make. For example, if you enter '9' as an option value, query 9 of 22 queries of tpc-h is created.
  

### Benchmarking tool for DB2 LUW

The **DB2** command line tool for benchmarking the SQL Statements along with summary file.

    db2batch -d dbname -a userid/pwd -f "filename" -z "output filename"

**-d dbname** : Database Name  
**-a userid/pwd** : Userid/password for connection to Database  
**-f filename** : file containing the SQL Statements to be benchmarked  
**-z outputfilename** : Summary file of the benchmarked results


**Sample Output of Summary File**


    * Timestamp: Tue Nov 13 2018 12:43:59 India Standard Time
    ---------------------------------------------
    
    
    * SQL Statement Number 1:
    
    
    create table t_demo (c1 int, c2 double, c3 varchar(4));
    
    
    * Elapsed Time is:       0.095165 seconds (complete)
    
    ---------------------------------------------
    
    
    * SQL Statement Number 2:
    
    
    insert into t_demo values (9223, 0.000000000000005, 'demo');
    
    * Elapsed Time is:       0.002255 seconds (complete)
    
    ---------------------------------------------
    
    
    * SQL Statement Number 3:
    
    
    insert into t_demo values (9272, 0.000000000000008, 'test');
    
    * Elapsed Time is:       0.006095 seconds (complete)
    
    ---------------------------------------------
    
    
    * SQL Statement Number 4:
    
    
    select * from t_demo;
    
    
    
    C1                   C2                     C3      
    -------------------- ---------------------- --------
    9223 		     5.00000000000000E-015  demo    
    9272 		     8.00000000000000E-015  test
    
    * 2 row(s) fetched, 2 row(s) output.
    
    * Prepare Time is:       0.000965 seconds
    * Execute Time is:       0.001830 seconds
    * Fetch Time is:         0.010875 seconds
    * Elapsed Time is:       0.013670 seconds (complete)
    
    * Summary Table:
    
    
    Type      Number      Repetitions Total Time (s) Min Time (s)   Max Time (s)   Arithmetic Mean Geometric Mean Row(s) Fetched Row(s) Output
    
    --------- ----------- ----------- -------------- -------------- -------------- --------------- -------------- -------------- -------------
    
    Statement           1           1       0.095165       0.095165       0.095165        0.095165       0.095165           0          0
    
    Statement           2           1       0.002255       0.002255       0.002255        0.002255       0.002255           0          0
    
    Statement           3           1       0.006095       0.006095       0.006095        0.006095       0.006095           0          0
    
    Statement           4           1       0.013670       0.013670       0.013670        0.013670       0.013670           2          2
    
    
    * Total Entries:              4
    * Total Time:                 0.117185 seconds
    * Minimum Time:               0.002255 seconds
    * Maximum Time:               0.095165 seconds
    * Arithmetic Mean Time:       0.029296 seconds
    * Geometric Mean Time:        0.011564 seconds


### Generated Timestamp for Every Insert Update on Row

    create table t_demo (
     c1 int, 
     c2 double, 
     c3 varchar(4),
     updated_time TIMESTAMP FOR EACH ROW ON UPDATE AS ROW CHANGE TIMESTAMP NOT NULL
     );

* Specifies that the column is a timestamp column for the table.  
* Value is generated for each row that is **inserted** and **updated**.
* Table can have only one  **ROW CHANGE TIMESTAMP** column.


### Information on Current SQL Execution 

```GET DIAGNOSTICS``` is used to get the information of current execution.

Returns :-

1. **DB2_RETURN_STATUS** :- Status value returned from the procedure.
2. **DB2_SQL_NESTING_LEVEL** :- Current level of recursion in effect.
3. **ROW_COUNT** :- Number of rows.
4. **DB2_TOKEN_STRING** :- Error or warning message tokens returned. 
5. **MESSAGE_TEXT** :- Error or warning message text returned.

**Syntax for Get Diagnostics**  

```
GET DIAGNOSTICS sql_variable1 = ROW_COUNT,
                sql_variable2 = DB2_RETURN_STATUS,
                sql_variable3 = DB2_SQL_NESTING_LEVEL,
                sql_variable4 = DB2_TOKEN_STRING,
                sql_variable5 = MESSAGE_TEXT;
```
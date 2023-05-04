Download Link: https://assignmentchef.com/product/solved-csci203-csci803-assignment-3
<br>
This assignment includes the tasks related to indexing of relational tables, implementation of data retrieval in PL/SQL and implementation of stored procedures and functions in PL/SQL.

This assignment consists of 4 tasks and specification of each task starts from a new page.

It is recommended to solve the problems before attending the laboratory classes in order to efficiently use supervised laboratory time.

A submission marked by Moodle as “late” is treated as a late submission no matter how many seconds it is late.

A policy regarding late submissions is included in the subject outline.

A submission of compressed files (zipped, gzipped, rared, tared, 7-zipped, lhzed, … etc) is not allowed. The compressed files will not be evaluated.

All files left on Moodle in a state “Draft(not submitted)” will not be evaluated.

An implementation that does not compile due to one or more syntactical errors scores no marks.

It is expected that all tasks included within <strong>Assignment 1</strong> will be solved <strong>individually without any cooperation</strong> with the other students.  If you have any doubts, questions, etc. please consult your lecturer or tutor during lab classes or office hours. Plagiarism will result in a <strong><u>FAIL</u></strong> grade being recorded for the assessment task.

<strong>Task 1</strong>

<h1>Prologue</h1>

Download the files dbcreate.sql and dbdrop.sql included in a section SAMPLE DATABASE. To drop a sample database, process a script dbdrop.sql. To create a sample database, process as script dbcreate.sql. It is strongly recommended to drop a sample database and to re-create it before implementation of each task.

Connect to Oracle database server and process the following SQL statement that saves a

query processing plan for a given SELECT statement in PLAN_TABLE.

EXPLAIN PLAN FOR SELECT ORDER_ID, ORDER_DATE FROM ORDERS;

Next, process the following SELECT statement to display a query processing plan stored in PLAN_TABLE.

SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);

Among the others, you should get the following results.

PLAN_TABLE_OUTPUT

——————————————————————————–

Plan hash value: 1275100350




—————————————————————————-

| Id  | Operation         | Name   | Rows  | Bytes | Cost (%CPU)| Time     |

—————————————————————————-

|   0 | SELECT STATEMENT  |        |   131 |  2882 |     3   (0)| 00:00:01 |

|   1 |  TABLE ACCESS FULL| ORDERS |   131 |  2882 |     3   (0)| 00:00:01 |

—————————————————————————-




A line TABLE ACCESS FULL| ORDERS in a plan given above indicates that a database system plans to access a table ORDERS to compute the query.




Next, create an index on the columns ORDER_ID and ORDER_DATE in a relational table ORDERS.




CREATE INDEX ORDERS_IDX ON ORDERS(ORDER_ID, ORDER_DATE);




Again, process the following SQL statements that save a query processing plan for the same SELECT statement as before in PLAN_TABLE and display a query processing plan stored in PLAN_TABLE.




EXPLAIN PLAN FOR SELECT ORDER_ID, ORDER_DATE FROM ORDERS; Among the others, you should get the following results.

PLAN_TABLE_OUTPUT

——————————————————————————–

Plan hash value: 2467194144




——————————————————————————-

| Id  | Operation        | Name       | Rows  | Bytes | Cost (%CPU)| Time     |

——————————————————————————-

|   0 | SELECT STATEMENT |            |   131 |  2882 |     1   (0)| 00:00:01 |

|   1 |  INDEX FULL SCAN | ORDERS_IDX |   131 |  2882 |     1   (0)| 00:00:01 |

——————————————————————————-

This time a database system plans to use an index ORDERS_IDX created a moment ago to process the same query. Note, a line INDEX FULL SCAN  | ORDERS_IDX in a plan given above means that a database system plans to horizontally traverse leaf level an index ORDERS_IDX to find the values in the columns ORDER_ID and ORDER_DATE.




<h2>Conclusions</h2>

EXPLAIN PLAN statement of SQL can be used to get information about a processing plan created by a query processor for a given SELECT statement. A query processing plan provides information on whether and index created earlier will be used for processing of SQL statement. We shall use EXPLAIN PLAN statement to check whether an index created to speed up SELECT statement will be used for processing of the statement.




To drop an index, process a statement




DROP INDEX ORDERS_IDX;




No report is expected from processing of SQL statements given above.




<h2>Problem</h2>

Your task is to find what indexes should be created to speed up processing of SELECT statements listed below. You are expected to create one index for one SELECT statement. To simplify the problem, assume that any index which is later on used by a query processor to speed up processing of SELECT statement will do.




<ul>

 <li>SELECT *</li>

</ul>

FROM ORDER_DETAIL

WHERE PRODUCT_NAME = ‘BOLT’ AND

QUANTITY &gt; 100;




<ul>

 <li>SELECT DISTINCT CATEGORY_NAME FROM PRODUCT;</li>

</ul>







<ul>

 <li>SELECT UNIT_PRICE FROM ORDER_DETAIL</li>

</ul>

WHERE QUANTITY IN (100, 200, 300) OR

DISCOUNT = 0.01;




<ul>

 <li>SELECT CATEGORY_NAME, SUPPLIER_NAME, COUNT(*)</li>

</ul>

FROM PRODUCT

GROUP BY CATEGORY_NAME, SUPPLIER_NAME;




<ul>

 <li>SELECT SUPPLIER_NAME, UNIT_PRICE</li>

</ul>

FROM PRODUCT

ORDER BY UNIT_PRICE, QUANTITY_PER_UNIT;




Implement SQL script solution1.sql such that for each one of SELECT statements given above the script performs the following actions.




<ul>

 <li>Find and list a query processing plan for SELECT statement without an index.</li>

 <li>Create an index.</li>

 <li>Find and list a query processing plan for SELECT statement with an index.</li>

 <li>Drop an index.</li>

</ul>




When ready process SQL script file solution1.sql and save a report from processing in a file solution1.lst.

Your report must include a listing of all PL/SQL statements processed. To achieve that put the following SQLcl commands:

SPOOL solution1

SET ECHO ON

SET FEEDBACK ON

SET LINESIZE 300

SET PAGESIZE 200

at the beginning of SQL script and

SPOOL OFF

at the end of SQL script.Deliverables

A file solution1.lst with a report from processing of a script file solution1.sql that lists query processing plans before and after indexing. A report must have no errors and it must list all SQL statements processed.

<u>                                                                                                                                                </u>

Task 2

Implement an anonymous PL/SQL block that lists order id (attribute ORDER_ID), company name that submitted order (attribute COMPANY_NAME), and order date (attribute ORDER_DATE) from the five most recently submitted orders.




To list information retrieved from a sample database use PL/SQL package DBMS_OUTPUT. It is explained in the Cookbook, Recipe 7.1 How to start programming

in     PL/SQL     how     to      use DBMS_OUTPUT      package.     Remember        about SET

SERVEROUTPUT ON at the beginning of a script file that contains your anonymous PL/SQL block.




Information retrieved must be listed in the following format.




Order id: 777

Order date: 26-APR-98

Company name: Golden Bolts Pty Ltd

=======================================

Order id: 666

Order date: 11-MAR-96

Company name: Lazy Lobster Seafood Corp

=======================================

…

and so on.




Your implementation must use at least one cursor and at least one exception handler. In fact, such constraints make your implementation easier.




To test your solution put an implemented anonymous PL/SQL block into SQL script file solution2.sql and process the script.

Your report must include a listing of all PL/SQL statements processed. To achieve that put the following SQLcl commands:

SPOOL solution2

SET SERVEROUTPUT ON

SET ECHO ON

SET FEEDBACK ON

SET LINESIZE 100

SET PAGESIZE 200

SET SERVEROUTPUT ON

at the beginning of SQL script and

SPOOL OFF

at the end of SQL script.

<h2>Deliverables</h2>

A file solution2.lst with a report from testing of an anonymous PL/SQL block implemented in this task. A report must have no errors and it must list all PL/SQL and SQL statements processed.

<u>                                                                                                                                                </u>

<h2>Task 3</h2>

Implement a stored PL/SQL procedure

INSERT_ORDER_DETAIL(order_id,product_name,unit_price,quantity,discount)

that inserts a row into a relational table ORDER_DETAIL and enforces the following consistency constraint on data entry into a relational table ORDER_DETAIL.

<em>A product can be ordered only if it is not discontinued. </em>

If the consistency constraint is satisfied insert and commit a row in ORDER_DETAIL table. Otherwise, use DBMS_OUTPUT PL/SQL package to display an error message when the consistency constraint is violated and do not insert a row.

When INSERT_ORDER_DETAIL    procedure        is         ready   create   SQL     script solution3.sql that stores the procedure in a data dictionary and tests the procedure with two EXECUTE statements. First, test the procedure for a product that is not discontinued and then test it again for a product that is discontinued. Any discontinued and not discontinued products used for testing will do.

Process SQL script solution3.sql and save a report from processing in a file solution3.lst.

Your report must include a listing of all PL/SQL statements processed. To achieve that put the following SQLcl commands:

SPOOL solution3

SET SERVEROUTPUT ON

SET ECHO ON

SET FEEDBACK ON

SET LINESIZE 100

SET PAGESIZE 200

SET SERVEROUTPUT ON

at the beginning of SQL script and

SPOOL OFF

at the end of SQL script.

<h2>Deliverables</h2>

A         file solution3.lst with             a          report   from    processing       of         SQL     script solution3.sql. A report must have no errors and it must list all PL/SQL and SQL statements processed.

<u>                                                                                                                                                </u>

<h2>Task 4</h2>

Implement a stored PL/SQL function

TOTAL_ORDERS(company_name)

that returns the total number of orders submitted by a given customer. Make a company name a parameter of a stored function. Assume that company name uniquely identifies each customer.




When ready, implement a script solution4.sql that stores the function in a data dictionary and tests a function. To test a function implement SELECT statements that lists company name (attribute COMPANY_NAME), address (attribute ADDRESS), and the total number of submitted orders for all companies that submitted more than 2 and less then 6 orders.

Process SQL script solution4.sql and save a report from processing in a file solution4.lst.

Your report must include a listing of all PL/SQL statements processed. To achieve that put the following SQLcl commands:

SPOOL solution4

SET SERVEROUTPUT ON

SET ECHO ON

SET FEEDBACK ON

SET LINESIZE 100

SET PAGESIZE 200

SET SERVEROUTPUT ON

at the beginning of SQL script and

SPOOL OFF

at the end of SQL script.

<h2>Deliverables</h2>

A         file solution4.lst with             a          report   from    processing       of         SQL     script solution4.sql. A report must have no errors and it must list all PL/SQL and SQL statements processed.
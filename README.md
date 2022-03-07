# DATABASE
What is the Cursor ?

What is CURSOR in PL/SQL?

A Cursor is a pointer to this context area. Oracle creates context area for processing an SQL statement which contains all information about the statement.
PL/SQL allows the programmer to control the context area through the cursor. A cursor holds the rows returned by the SQL statement. The set of rows the cursor holds is referred as active set. These cursors can also be named so that they can be referred from another place of the code.
In this tutorial you will learn-
•	Implicit Cursor
•	Explicit Cursor
•	Cursor Attributes
•	FOR Loop Cursor statement
The cursor is of two types.
•	Implicit Cursor
•	Explicit Cursor
Implicit Cursor
Whenever any DML operations occur in the database, an implicit cursor is created that holds the rows affected, in that particular operation. These cursors cannot be named and, hence they cannot be controlled or referred from another place of the code. We can refer only to the most recent cursor through the cursor attributes.
Explicit Cursor
Programmers are allowed to create named context area to execute their DML operations to get more control over it. The explicit cursor should be defined in the declaration section of the PL/SQL block, and it is created for the ‘SELECT’ statement that needs to be used in the code.
Below are steps that involved in working with explicit cursors.
•	Declaring the cursor Declaring the cursor simply means to create one named context area for the ‘SELECT’ statement that is defined in the declaration part. The name of this context area is same as the cursor name.
•	Opening CursorOpening the cursor will instruct the PL/SQL to allocate the memory for this cursor. It will make the cursor ready to fetch the records.
•	Fetching Data from the CursorIn this process, the ‘SELECT’ statement is executed and the rows fetched is stored in the allocated memory. These are now called as active sets. Fetching data from the cursor is a record-level activity that means we can access the data in a record-by-record way. Each fetch statement will fetch one active set and holds the information of that particular record. This statement is same as ‘SELECT’ statement that fetches the record and assigns to the variable in the ‘INTO’ clause, but it will not throw any exceptions.
•	Closing the CursorOnce all the record is fetched now, we need to close the cursor so that the memory allocated to this context area will be released.
Syntax:
DECLARE
CURSOR <cursor_name> IS <SELECT statement^>
<cursor_variable declaration>
BEGIN
OPEN <cursor_name>;
FETCH <cursor_name> INTO <cursor_variable>;
.
.
CLOSE <cursor_name>;
END;
/*
•	In the above syntax, the declaration part contains the declaration of the cursor and the cursor variable in which the fetched data will be assigned.
•	The cursor is created for the ‘SELECT’ statement that is given in the cursor declaration.
•	In execution part, the declared cursor is opened, fetched and closed.
Cursor Attributes
Both Implicit cursor and the explicit cursor has certain attributes that can be accessed. These attributes give more information about the cursor operations. Below are the different cursor attributes and their usage.
Cursor Attribute	Description
%FOUND	It returns the Boolean result ‘TRUE’ if the most recent fetch operation fetched a record successfully, else it will return FALSE.
%NOTFOUND	This works oppositely to %FOUND it will return ‘TRUE’ if the most recent fetch operation could not able to fetch any record.
%ISOPEN	It returns Boolean result ‘TRUE’ if the given cursor is already opened, else it returns ‘FALSE’
%ROWCOUNT	It returns the numerical value. It gives the actual count of records that got affected by the DML activity.
Explicit Cursor Example:
In this example, we are going to see how to declare, open, fetch and close the explicit cursor.
We will project all the employee’s name from emp table using a cursor. We will also use cursor attribute to set the loop to fetch all the record from the cursor.
*/ 
DECLARE
CURSOR guru99_det IS SELECT emp_name FROM emp;
lv_emp_name emp.emp_name%type;

BEGIN
OPEN guru99_det;

LOOP
FETCH guru99_det INTO lv_emp_name;
IF guru99_det%NOTFOUND
THEN
EXIT;
END IF;
Dbms_output.put_line(‘Employee Fetched:‘||lv_emp_name);
END LOOP;
Dbms_output.put_line(‘Total rows fetched is‘||guru99_det%R0WCOUNT);
CLOSE guru99_det;
END:
/
Output
Employee Fetched:BBB
Employee Fetched:XXX
Employee Fetched:YYY 
Total rows fetched is 3




Code Explanation:
•	Code line 2: Declaring the cursor guru99_det for statement ‘SELECT emp_name FROM emp’.
•	Code line 3: Declaring variable lv_emp_name.
•	Code line 5: Opening the cursor guru99_det.
•	Code line 6: Setting the Basic loop statement to fetch all the records in the ’emp’ table.
•	Code line 7: Fetches the guru99_det data and assign the value to lv_emp_name.
•	Code line 9: Using the cursor attribute ‘%NOTFOUND’ to find whether all the record in the cursor is fetched. If fetched then it will return ‘TRUE’ and control will exit from the loop, else the control will keep on fetching the data from the cursor and print the data.
•	Code line 11: EXIT condition for the loop statement.
•	Code line 12: Print the fetched employee name.
•	Code line 14: Using the cursor attribute ‘%ROWCOUNT’ to find the total number of records that got affected/fetched in the cursor.
•	Code line 15: After exiting from the loop the cursor is closed and the memory allocated is set free.
FOR Loop Cursor statement
“FOR LOOP” statement can be used for working with cursors. We can give the cursor name instead of range limit in the FOR loop statement so that the loop will work from the first record of the cursor to the last record of the cursor. The cursor variable, opening of cursor, fetching and closing of the cursor will be done implicitly by the FOR loop.
Syntax:
DECLARE
CURSOR <cursor_name> IS <SELECT statement>;
BEGIN
  FOR I IN <cursor_name>
  LOOP
  .
  .
  END LOOP;
END;
•	In the above syntax, the declaration part contains the declaration of the cursor.
•	The cursor is created for the ‘SELECT’ statement that is given in the cursor declaration.
•	In execution part, the declared cursor is setup in the FOR loop and the loop variable ‘I’ will behave as cursor variable in this case.
Oracle Cursor for Loop Example:
In this example, we will project all the employee name from emp table using a cursor-FOR loop.
DECLARE
CURSOR guru99_det IS SELECT emp_name FROM emp; 
BEGIN
FOR lv_emp_name IN guru99_det
LOOP
Dbms_output.put_line(‘Employee Fetched:‘||lv_emp_name.emp_name);
END LOOP;
END;
/
Output
Employee Fetched:BBB 
Employee Fetched:XXX
Employee Fetched:YYY
______________________________________________________________________________________________________________________________________________________________________________
  ___________________________________________________________________________________________________________________________________________________________________________
****** EXAMPLE OF CURSOR **********
<!-- CREATED DATABASE  -->

  
CREATE TABLE emp (
  empno    NUMBER(4) CONSTRAINT pk_emp PRIMARY KEY,
  ename    VARCHAR2(10),
  job      VARCHAR2(9),
  mgr      NUMBER(4),
  hiredate DATE,
  sal      NUMBER(7,2),
  comm     NUMBER(7,2),
  deptno   NUMBER(2)
);

<!-- INSERTED VALUES -->
  
INSERT INTO emp VALUES (7369,'SMITH','CLERK',7902,to_date('17-12-1980','dd-mm-yyyy'),800,NULL,20);
INSERT INTO emp VALUES (7499,'ALLEN','SALESMAN',7698,to_date('20-2-1981','dd-mm-yyyy'),1600,300,30);
INSERT INTO emp VALUES (7521,'WARD','SALESMAN',7698,to_date('22-2-1981','dd-mm-yyyy'),1250,500,30);
INSERT INTO emp VALUES (7566,'JONES','MANAGER',7839,to_date('2-4-1981','dd-mm-yyyy'),2975,NULL,20);
INSERT INTO emp VALUES (7654,'MARTIN','SALESMAN',7698,to_date('28-9-1981','dd-mm-yyyy'),1250,1400,30);
INSERT INTO emp VALUES (7698,'BLAKE','MANAGER',7839,to_date('1-5-1981','dd-mm-yyyy'),2850,NULL,30);
INSERT INTO emp VALUES (7782,'CLARK','MANAGER',7839,to_date('9-6-1981','dd-mm-yyyy'),2450,NULL,10);
INSERT INTO emp VALUES (7788,'SCOTT','ANALYST',7566,to_date('13-JUL-87','dd-mm-rr')-85,3000,NULL,20);
INSERT INTO emp VALUES (7839,'KING','PRESIDENT',NULL,to_date('17-11-1981','dd-mm-yyyy'),5000,NULL,10);
INSERT INTO emp VALUES (7844,'TURNER','SALESMAN',7698,to_date('8-9-1981','dd-mm-yyyy'),1500,0,30);
INSERT INTO emp VALUES (7876,'ADAMS','CLERK',7788,to_date('13-JUL-87', 'dd-mm-rr')-51,1100,NULL,20);
INSERT INTO emp VALUES (7900,'JAMES','CLERK',7698,to_date('3-12-1981','dd-mm-yyyy'),950,NULL,30);
INSERT INTO emp VALUES (7902,'FORD','ANALYST',7566,to_date('3-12-1981','dd-mm-yyyy'),3000,NULL,20);
INSERT INTO emp VALUES (7934,'MILLER','CLERK',7782,to_date('23-1-1982','dd-mm-yyyy'),1300,NULL,10);
COMMIT;

<!--  Explicit cursor method -->

DECLARE 
   e_empno emp.empno%type; 
   E_Ename emp.ename%type; 
   E_job emp.job%type; 

   CURSOR c_emp is 
      SELECT empno,ENAME,job
      FROM emp; 
BEGIN 
   OPEN c_emp; 
   LOOP 
   FETCH c_emp into e_empno, E_Ename, E_job; 
      EXIT WHEN c_emp%notfound;
      dbms_output.put_line('No' || '    ' || 'Name' || ' ' || 'job');
      dbms_output.put_line(e_empno || ' ' || E_Ename || ' ' || E_job); 
   END LOOP; 
   CLOSE c_emp; 
END; 



<!-- for_loop cursors method -->

declare 
cursor cur_emp
is select * from emp
where deptno='30';

BEGIN
for r_cur in cur_emp
LOOP
dbms_output.put_line('Emp Name:'||r_cur.ename);
dbms_output.put_line('Emp No:'||r_cur.empno);
END LOOP;
END;


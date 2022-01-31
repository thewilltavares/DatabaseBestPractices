#**What is "Idempotent", Anyway?**
An "idempotent" SQL script is a script that you can run once or an infinite number of times and get the same result without error every time.  For example, an idempotent INSERT script checks for the existence of a row before inserting it.  An idempotent ADD COLUMN script checks for the existence of a column before adding it to a table.

If you are having trouble making your script be idempotent, ask a DBA for help.  Also, see <a href="https://www.sqlservercentral.com/steps/idempotent-ddl-scripts-that-always-achieve-the-same-result-making-changes-only-once-stairway-to-exploring-database-metadata-level-6">this article by "Phil Factor"</a>.

#**Example of Adding a Row to a Table**
<PRE>
INSERT
  INTO dbo.User (Name)
SELECT 'My New User Name' AS Name
 WHERE NOT EXISTS (
       SELECT 'x'
         FROM dbo.User
        WHERE Name = 'MMy New User Name') ;
</PRE>

#**Example of Adding a Column to a Table**
<PRE>
IF NOT EXISTS (
    SELECT 'x'
      FROM INFORMATION_SCHEMA.COLUMNS
     WHERE table_name = 'User' 
       AND column_name = 'PrimaryPhone')
BEGIN
    ALTER TABLE User ADD PrimaryPhone INT NULL ;
END ;</PRE>

#**Example of Dropping an Object**
<PRE>
IF Object_Id('dbo.NeedlessTable','U') IS NOT NULL DROP TABLE dbo.NeedlessTable ;
</PRE>
Note that the second parameter of the <code>Object_Id()</code> function call is the object type.  Tables are object type 'U', procedures are object type 'P', etc.  For a list of common object types, see the next section.  For a complete list go to:  https://docs.microsoft.com/en-us/sql/relational-databases/system-catalog-views/sys-objects-transact-sql?view=sql-server-2017

#**Common Object Type Identifiers**
These object type identifiers are needed when using the <code>Object_Id()</code> function to test for the existence of an object before attempting to create, modify, or drop the object:
<table>
<tr><td><b>Type Identifier</b></td><td><b>Type Description</b></td></tr>
<tr><td>C</td><td>CHECK constraint</td></tr>
<tr><td>D</td><td>Default or DEFAULT constraint</td></tr>
<tr><td>F</td><td>FOREIGN KEY constraint</td></tr>
<tr><td>FN</td><td>Scalar function</td></tr>
<tr><td>IF</td><td>In-lined table function</td></tr>
<tr><td>P</td><td>Stored procedure</td></tr>
<tr><td>PK</td><td>PRIMARY KEY constraint</td></tr>
<tr><td>SN</td><td>Synonym</td></tr>
<tr><td>TF</td><td>Table function</td></tr>
<tr><td>TR</td><td>SQL DML trigger</td></tr>
<tr><td>TT</td><td>Table type</td></tr>
<tr><td>U</td><td>User table</td></tr>
<tr><td>V</td><td>View</td></tr>
</table>


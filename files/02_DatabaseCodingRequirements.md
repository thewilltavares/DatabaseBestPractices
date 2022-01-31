## **Data Types**
<ul>
<li>Variable and parameter data types should match the corresponding table columns, e.g. if a DATETIME table column <code>StartDate</code> is to be compared with the variable <code>@StartDate</code>, then the variable should also be DATETIME, not SMALLDATETIME.</li>
<li>Use VARCHAR for character fields; CHAR should only be used when a fixed length is required.  Do not invite ridicule by assigning a data type of VARCHAR(1).</li>
<li>Do not use NCHAR or NVARCHAR unless Unicode support is required.</li>
<li>Use INT, not SMALLINT or TINYINT.</li>
<li>Use BIGINT only when necessary.</li>
<li>Use DATE or DATETIME, not SMALLDATETIME.</li>
<li>Do not use TEXT datatype.  Use VARCHAR(max) for large character fields instead of TEXT datatype.</li>
</ul>

## **Object Definition Scripts**
<ul>
<li>All object definition scripts should be under source control.</li>
<li>Each object definition script should have the same name as the object being defined.</li>
<li>No objects other than the object the script is named for should have a DROP, CREATE, or GRANT command anywhere in the script.  This includes scripts for renamed objects.</li>
<li>Begin each object definition script with some type of “DROP IF EXISTS” command, followed by a GO.  There are many versions of this command.  Here is a terse example:
<br><code>IF Object_Id('dbo.uspAircraft_Get','P') IS NOT NULL DROP PROCEDURE dbo.uspAircraft_Get ;<br>
GO</code><br><br>
Here is a list of some object type identifiers that developers are likely to use with the <code>Object_Id()</code> function (in the example above, a procedure is being conditionally dropped):
<table>
<tr><td><b>Type Identifier</b></td><td><b>Type Description</b></td></tr>
<tr><td>FN</td><td>Scalar function</td></tr>
<tr><td>IF</td><td>In-lined table function</td></tr>
<tr><td>P</td><td>Stored procedure</td></tr>
<tr><td>SN</td><td>Synonym</td></tr>
<tr><td>TF</td><td>Table function</td></tr>
<tr><td>TR</td><td>SQL DML trigger</td></tr>
<tr><td>TT</td><td>Table type</td></tr>
<tr><td>U</td><td>User table</td></tr>
<tr><td>V</td><td>View</td></tr>
</table><br></li>
<li>Include heading comments and a modification log in a comment block near the beginning of each object definition.  There is not a standard format, but the modification log should always include the date, the developer name, and a description of what was modified.  Here is a fictitious example of a procedure header with a modification log:<br>
<PRE>
/****************************************************************************************
PROCEDURE:   uspUser_Get
DESCRIPTION: Retrieves basic data about a user.
DATABASE:    Prod
TEST STRING: EXEC dbo.uspUser_Get @UserID = 9343 ;<br>
CHANGE CONTROL:
Initials  Date        Description
=========================================================================================
XXX       06/25/2018  Created
XXX       07/16/2018  Added ActiveFlag to result set (User Story 9999).
*****************************************************************************************/</PRE>(Also see the "Stored Procedures and User-Defined Functions" topic for more about heading comments.)</li><br>
<li>End the object definition with a GO statement.</li>
<li>If any GRANTs are necessary for the object, then include them at the end of the script.  Follow the GRANT commands with a GO statement.</li>
</ul>

## **Use of Table and Column Aliases**
<ul>
<li>Prefix every table or column alias declaration with the “AS” keyword.  This improves readability.  Example:  use “SELECT con.Name AS ContactName FROM dbo.Contact AS con“ instead of “SELECT con.Name ContactName FROM dbo.Contact con“.</li>
<li>Use meaningful table alias names.  For example, “usr” for User or “tov” for TableOfValues are good alias names.  Please do not alias the first table as “a”, the second table as “b”, etc.</li>
<li>Always use a table alias to qualify column names.  Do not use the entire table name.</li>
<li>If there is more than one table in the FROM clause, then prefix every column reference with a table alias.  This will prevent ambiguous column names, which is especially important when columns are added to tables later.</li>
<li>DECLARE all SQL variables at the beginning of an object definition or DML script.  Do not DECLARE variables after the first executable code or especially inside IF blocks or LOOPs.</li>
</ul>

## **Hard-coded database references**
<ul>
<li>DON'T</li>
<li>When accessing objects in another database, use a synonym.  Do not use three or four part object qualifiers.</li>
<li>Synonyms should be named syn<three character database abbreviation>_<full remote object name, no abbreviations>, for example synFOI_AIRPORT is the standard synonym name for the FOIAVIFS.dbo.AIRPORT table.  If you are not sure of the correct database abbreviation, ask a DBA (do not look at prior synonym names, since past naming has been inconsistent).</li>
<li>When writing a script to create a new synonym, call the msdb.dbo.uspSynonym_Create procedure so that the synonym creation script will be portable to any environment including production.  Here is an example of how to use uspSynonym_Create to create the FOIAVIFS.dbo.synFOM_Logbook_ByTripleg synonym:
<PRE>
DECLARE @SynonymCurrentDB VARCHAR(128) = Db_Name() ;
EXEC msdb.dbo.uspSynonym_Create
    @TargetObjectName     = 'Logbook_ByUser',     --name of the object referenced by the synonym
    @TargetProductionDB   = 'User_MetaData',      --name of the referenced object's production database
    @TargetDBAbbreviation = 'USR',                --abbreviation of referenced object's production database
    @SynonymProductionDB  = 'USRDB',              --name of synonym's production database
    @SynonymCurrentDB     = @SynonymCurrentDB,    --name of database in which to create the synonym
    @DebugFlag            = 0 ;                   --set @DebugFlag = 1 to PRINT the generated synonym creation script</PRE></li>
</ul>

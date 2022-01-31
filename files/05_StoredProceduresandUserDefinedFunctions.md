## **Naming Stored Procedures**
<ul>
<li>The naming convention for a stored procedure is: <code>usp<TableName>_<Function></code><br>
<br>
Examples:<br>
<code>uspContact_Get</code><br>
<code>uspContact_GetPrimaryPhone</code><br>
<code>uspContact_GetBirthdate</code><br>
<br>
</li>
<li>No blanks may be embedded in a stored procedure name.</li>
<li>The standard "functions" in the name are: Upd, Ins, Get, Del.</li>
<li>Upsert procs (combined Insert and Update) should generally not be used, but when it makes sense, the "function" is Ups.  If you feel this is a necessary course of action, please consult the DBAs before doing so.</li>
<li>The names of stored procedures defined solely for use with reports should follow the format: <code>uspRPT<ReportName></code>.</li>
<li>Processing routines (those that process data with limited inputs and outputs) do not require an appended function suffix, e.g. <code>uspInvoiceProcessing</code>.</li>
</ul>

## **Naming User Defined Functions (UDFs)**
<ul>
<li>The naming convention for a user-defined function is: <code>udf<function></code>.</li>
<li>No blanks may be embedded in the function name.</li>
<li>A function name should give a clear indication of the purpose of the UDF.</li>
</ul>

## **Parameters**
<ul>
General syntax example:<br>
<PRE>
CREATE OR ALTER PROCEDURE uspMyTable_Get
    @Parm1 INT,
    @Parm2 VARCHAR(50)   = ‘Default’,
    @Parm3 DECIMAL(10,2) = NULL,
    @Parm4 INT OUTPUT

AS ...</PRE>
<br>
<li>Do not use a DROP IF EXISTS statement at the top of the script, rather use CREATE OR ALTER as the beginning of the first line as shown in the example.</li>
<li>Do not provide default values for required parameters (as demonstrated by @Parm1 above).</li>
<li>Provide default values for all optional parameters (as demonstrated by @Parm2 and @Parm3 above).</li>
<li>List all OUTPUT parameters at the end of the parameter list (except where maintenance would result in breaking existing interfaces, required for all new code).</li>
<li>Follow additional rules defined for local variable declarations.</li>
</ul>

## **Header Comment Block**<ul>
<ul>
<li>All object definitions scripts are required to have a header with a revision history section.</li>
<li>All modifications must be noted in the comment header including the date of the change, developer making the change and a brief description of the modification.</li>
<li>Comments should be listed in order of occurrence with the latest at the end of the list.</li>
<li>Keep header comments brief.</li>
<li>This is a fictitious example of a header comment block for a procedure:<br>
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
*****************************************************************************************/</PRE></li>
</ul>

## **Local Variables**
<ul>
<li>All local variables should be declared immediately following the comment header section using a single <code>DECLARE</code> statement.</li>
<li>Local variable name and datatype should match the corresponding column name, where applicable.</li>
<li>Avoid using abbreviations where possible.</li>
<li>Use “Proper” case capitalization; do not use all UPPERCASE or all lowercase.</li>
<li>All on/off flags should use <code>BIT</code> datatype; others use <code>CHAR(1)</code> or <code>INT</code> as appropriate.</li>
<li>All <code>BIT</code> datatype variable names should end in <code>Flag</code>.</li>
<li><code>TABLE</code> type variables should be defined last, separately from the other variables.   Each <code>TABLE</code> variable should have its own <code>DECLARE</code> statement.
</ul>


## **Temporary Tables**
<ul>
<li>All temporary tables to be used within the code should be created immediately following local variable declarations, unless temporary table creation is dependent on other executable statements.</li>
<li>All temporary tables must be dropped before ending the code.  (Note that for purposes of debugging, it is also often necessary to put a conditional drop of a temporary table immediately before it is created.)</li>
<li>Indexes may be created on a temporary table as necessary to improve performance.</li>
</ul>

## **Cursors**
<ul>
<li>Cursors should only be used when necessary.  If you think you should use a cursor, double-check with a DBA as they may know a more efficient way to accomplish the same thing without a cursor. </li>
<li>End all cursor names with <code>Cursor</code>.</li>
<li>Use keywords <code>LOCAL</code> and <code>FAST_FORWARD</code> in cursor declarations when possible (see <a href="https://sqlperformance.com/2012/09/t-sql-queries/cursor-options">this article</a> by Aaron Bertrand if you want to know why).</li>
<li>All cursors should be defined inline (i.e. just prior to the <code>OPEN</code> statement), not at the beginning of the code with other variable declarations.</li>
<li>All cursors must be closed and deallocated after use, using the <code>CLOSE</code> and <code>DEALLOCATE</code> commands.</li>
<li>Do not use a WHILE loop to avoid using a CURSOR.  If it can't be done in a set-based operation (see DBA), then CURSORs were designed for this, and usually work better than a WHILE.</li>
</ul>

## **BEGIN/END Blocks**
<ul>
<li>The trailing <code>END</code> statement for a block should be left-aligned with its corresponding <code>BEGIN</code>.</li>
<li>All statements within an <code>IF…THEN</code> or <code>WHILE</code> loop should be encapsulated in a <code>BEGIN</code>/<code>END</code> block, even if only one statement is being executed.  Rare exceptions to this rule are allowed for readability of consecutive <code>IF</code> statements.</li>
</ul>

## **Permissions** 
<ul>
<li>Permissions should be granted to explicit SQL Server roles, not public.</li>
</ul>

## **DML Formatting**
Note:  DML is an abbreviation for Data Modification Language or Data Manipulation Language, depending on who you ask.
<ul>
<li>The primary keyword (SELECT, INSERT, UPDATE, DELETE) should always be left-aligned with the previous code statement (indented once if immediately preceded by a BEGIN other than the initial BEGIN statement).</li>
<li>Conditional AND/ORs (used within parenthesis) should be aligned so that it is clearly apparent what criteria are linked to what additional criteria.</li>
<li>Aliases should be used whenever possible and should be lower case.</li>
<li>Alias declarations should be preceded by the AS keyword.</li>
<li>For the sake of readability, only criteria used to match rows should be referenced in JOIN criteria.  Other criteria should be placed in the WHERE clause.  The exception to this guideline is when there is a LEFT JOIN and it is syntactically much easier to put all criteria into the LEFT JOIN rather than handle a lot of IS NULL conditions in the WHERE clause.</li>
</ul>

## **Reserved Words**
<ul>
<li>Reserved words should be in ALL CAPS.</li>
<li>Reserved words should be avoided as table, column, and alias names.</li>
</ul>

## **Comments In Code**
<ul>
<li>Programmers are expected to use their best judgement when commenting code logic; the goal is for readability and brevity, with an eye towards future maintenance.</li>
<li>Obsolete code should be deleted, not commented out, since source control allows for previous versions to be retrieved.</li>
<li>Single-line comments should be written using the double dash (--) notation.</li>
<li>Multi-line comments may use the double dash (--) notation on each line or using block (/* … */) comment delimiters.</li>
</ul>

## **Comparisons**
<ul>
<li>Use <code><></code> instead of <code>NOT =</code> or <code>!=</code>.  Use of <code>!=</code> is not deprecated (yet) but it is non-ANSI.</li>
<li>Use BETWEEN instead of @Variable >= Column1 AND @Variable <= Column2.</li>
</ul>

## **SELECT vs SET**
<ul>
<li>When assigning a static value to a local variable, use SET.</li>
<li>When assigning static values to multiple local variables, use individual SET statements except when setting local variables from system functions that my be “zeroed out” by using multiple statements, e.g. <code>SELECT @Error = @@ERROR</code>.</li>
<li>When assigning multiple values from a table, use a single SELECT statement.</li>
</ul>

## **GOTO Statements**
<ul>
<li>DON'T</li>
<li>Contact the DBA Group for proper direction.</li>
</ul>

## **SELECT * Queries**
<ul>
<li>DON'T</li>
<li>Never use <code>SELECT *</code> syntax except in an ad-hoc query that will not be consumed by a process.  In particular, view and procedure definitions should never contain <code>SELECT *</code>.</li>
</ul>

## **Dynamic SQL**
<ul>
<li>In general... DON'T</li>
<li>Contact the DBA Group for proper direction.  Occasionally there are valid reasons to use dynamic SQL, but there are always good reasons not to.
</li>
</ul>

## **WITH (NOLOCK)**

<ul>
<li>In general... DON'T</li>
<li>Contact the DBA Group if you have questions.</li>
</ul>
The best way to prevent deadlocks and blocking issues is to write efficient queries and make good use of indexes.

Use of NOLOCK is usually a very bad idea.  If you want more information about why NOLOCK is bad, then read <a href="https://blogs.msdn.microsoft.com/davidlean/2009/04/05/sql-server-nolock-hint-other-poor-ideas/">this article</a> or the answer to <a href="https://stackoverflow.com/questions/1452996/is-the-nolock-sql-server-hint-bad-practice">this forum question</a>.  

There are situations where NOLOCK is appropriate, for example:  the query result is for your own purposes and you don't care if it is inaccurate; there is a long-running process inside of a transaction and you need to see how much progress it is making; or, there is a <u>known</u> deadlock or blocking issue that can't be solved any other way.  

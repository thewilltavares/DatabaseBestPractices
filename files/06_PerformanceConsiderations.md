## **Cursors**
<ul>
<li>Never use a cursor if you can find a way to do the same thing in a set-based operation.  Talk to a DBA if you are unsure how to convert cursor procedural logic to set logic.</li>
</ul>

##**EXISTS and NOT EXISTS**
See the answer to <a href="https://stackoverflow.com/questions/7082449/exists-vs-join-and-use-of-exists-clause">this forum question about how EXISTS works</a> and <a href="https://sqlperformance.com/2012/12/t-sql-queries/left-anti-semi-join">this article by Aaron Bertrand that shows why NOT EXISTS is usually better</a>.  Also, here is a <a href="https://us2.campaign-archive.com/?e=5eb161f38d&u=9082566fb63d87be35c0662bc&id=f2d7cdaec9">Brent Ozar/Erik Darling blog post</a> on the subject and an <a href="https://www.sqlservercentral.com/blogs/not-exists-vs-not-in">article by Gail Shaw</a>.
<ul>
<li>In general, avoid using a JOIN or IN/NOT IN if EXISTS/NOT EXISTS will work.  </li>
<li>Use EXISTS instead of a JOIN to a secondary table if you only need to restrict the rows returned and don’t need any data from the secondary table.  EXISTS is faster and prevents multiple rows from being returned if there is a match with more than one secondary table row.</li>
<li>Use EXISTS instead of IN, because EXISTS is faster.</li>
<li>Use NOT EXISTS instead of LEFT JOIN… WHERE <column> IS NULL.  NOT EXISTS is faster and is considered best practice.</li>
<li>Use NOT EXISTS instead of NOT IN.  NOT EXISTS is faster and there can be unexpected behavior when there is a NULL in the NOT IN list (try executing <code>SELECT '1 is not 0 or NULL' AS [Result] WHERE 1 NOT IN (0,NULL) ;</code>).</li>
</ul>

##**WHERE Clause**
<ul>
<li>Avoid implicit type conversions in the WHERE clause.  In other words, make sure that data types are the same when comparing two values.  Implicit type conversions prevent index use and increase CPU time.  For example, do not DECLARE a variable as a DATE data type and then compare it with a DATETIME database column in a WHERE clause.  Do not compare a FLOAT with an INT, etc.</li>
<li>Avoid calling a function in a WHERE clause.</li>
<li>If there is a computed constant in a WHERE clause, DECLARE a variable and SET it to the computed constant.  Then, reference the variable in the WHERE clause instead of putting the computation in the WHERE clause.  Referencing a variable is much faster.  This guideline is often relevant to GetDate() and GetUtcDate() function calls.</li>
</ul>

##**User-Defined Functions**
<ul>
<li>Avoid calling a user-defined function anywhere in a query, if the query plan shows the function will be executed many times.</li>
</ul>

##**Table Variables**
<ul>
<li>Only use table variables for small sets of data, because the SQL Server optimizer does not compute statistics for table variables.  This means that the expected row count for a table variable is always 1 and a table variable will always be put into a join’s outer loop.</li>
</ul>

## **Use Object Schema**
<ul>
<li>Qualify every object name with a schema.  For example, use <code>FROM dbo.Contact AS con</code> instead of <code>FROM Contact AS con</code>, or <code>EXEC dbo.uspDoSomething</code> instead of <code>EXEC uspDoSomething</code>.  This allows plan re-use and makes compilation a little faster.  Omitting the schema prevents plan re-use (see <a href="https://www.red-gate.com/simple-talk/blogs/why-you-should-always-use-schema-name-in-queries/">the blog at this link</a>).  SQL Server has to search every schema for the object name if no schema is specified (see <a href="https://dzone.com/articles/7-simple-tips-to-boost-the-performance-of-your-sql">the article at this link</a> and <a href="https://www.youtube.com/watch?v=ztimAjvlhRk">the video at this link</a>.)</li>
</ul>


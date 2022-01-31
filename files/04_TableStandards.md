## **Naming**
<ul>
<li>Tables should be named using proper case (e.g. <code>TableOfValues</code>).</li>
<li>Underscores should not be used in table names.</li>
<li>Table names should be singular, since a table is a collection of individual records (e.g. <code>Contract</code> instead of <code>Contracts</code>).</li>
</ul>

## **Column Naming**
<ul>
<li>Columns should be named using proper case (e.g. <code>ContactID, SalesDirectorID</code>)</li>
<li>Do not use underscores in column names</li>
<li>Use abbreviations sparingly, since Intellisense allows us be descriptive without sacrificing coding speed.  For example:<br>
<br>
<code>CommunicationTypeID</code> instead of <code>CommTypID</code><br>
<code>SalesDirectorID</code> instead of <code>SDID</code> or <code>SlsDrID</code><br>
</br></li>
</ul>

## **Required Columns**
<ul>
<li>All tables in which an end user can modify data via a UI should include the following columns <u>or an equivalent</u>:<br>
<br>

<code>CreateUser  varchar(50) NOT NULL,</code>
<code>CreateDate  datetime NOT NULL,</code>
<code>RevisionUser varchar(50) NOT NULL,</code>
<code>RevisionDate datetime NOT NULL</code>
<br>
</li>
</ul>

## **Keys (Primary and Foreign)**
<ul>
<li>All physical tables should have a single identifying primary key column whose name matches that of the table appended by "ID".  This column should be defined as an INT datatype.  With the exception of some code tables, this column should be defined as IDENTITY.  The primary key can be defined inline with the table definition (script) or via an alter command. In most cases, the primary key constraint should be clustered.  Primary keys must be explicitly named as follows:<br>
<br>
<code>PK_TableName</code><br>
<br>
</li>
<li>All columns referenced against another table should be defined as a foreign key constraint, where possible. Foreign keys should be named as follows:<br>
<br>
<code>FK_ParentTableName_ChildTableName</code><br>
<br>
</li>
</ul>

## **Indexes**
<ul>
<li>All columns constrained by foreign keys should be indexed.  An index on a foreign key column should be named as follows:<br>
<br>
<code>XIFColumnName</code><br>
<br>
</li>
<li>All columns frequently accessed as part of criteria should be indexed (secondary index).  A secondary index should be named as follows:<br>
<br>
<code>XIColumnName</code><br>
<br>
</li>
<li>Columns frequently queried together should be combined in a composite index.  A composite index should be named like a secondary index with a brief, reasonable name for the column grouping.  If a column is frequently queried but not useful for discriminating rows to be retrieved, then it should be added to the index as an INCLUDE column.
</li>
</ul>

## **Bit data types**
<ul>
<li>All columns defined as BIT datatype should end in the keyword "Flag".</li>
</ul>

## **Permissions**
<ul>
<li>No explicit permissions should be granted on a table; data access and permissions are granted solely through stored procedures.</li>
</ul>

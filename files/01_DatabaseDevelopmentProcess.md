<ol>
<li>Gather process requirements.</li>
<li>If a database schema modification is identified during the design process and a DBA has time, then meet with DBAs to discuss modifications before any stored procedure, API, or UI coding.  Examples of changes that are important to discuss with a DBA are:  new tables, new views, unusually complex or slow queries, cursor loops, and interfaces between processes or systems.  Also meet with DBAs if there are any questions about naming of tables, views, synonyms, procedures, or functions.</li>
<li>Write scripts for database modifications.</li>
<li>Unit-test changes in development environment.</li>
<li>Promote changes to QA** environment.</li>
<li>System-test changes in QA environment.</li>
<li>Request SQL code review.</li>
<li>DBA(s) review unit-tested and system-tested code in QA environment.</li>
<li>Following a successful code review, check-in changes to QA and request promotion to UAT.</li>
<li>DBA(s) promote changes to UAT environment.  A DBA may reformat the code if necessary before promoting it to UAT.</li>
<li>User-test changes in UAT environment.</li>
<li>If a problem is found in UAT, then the defective script (or entire feature) is demoted (moved) to QA or DEV for remediation.</li>
<li>At least two work days prior to a planned deployment to the production environment, submit a deployment request.  DBA(s) will perform a final review of items to be deployed and then deploy changes in the next release cycle.</li>
</ol>
<br>
** It is understood that some systems lack a DEV, QA, or UAT environment.  In those cases we will improvise a process that utilizes only the available environment(s) but still includes a DBA code review prior to user acceptance testing and deployment.
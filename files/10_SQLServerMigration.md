## **Copy Users**

```
SELECT 'IF(SUSER_ID('+QUOTENAME(SP.name,'''')+') IS NULL)BEGIN CREATE LOGIN '+QUOTENAME(SP.name) +
      CASE WHEN SP.type_desc = 'SQL_LOGIN'
           THEN ' WITH PASSWORD = '+CONVERT(NVARCHAR(MAX),SL.password_hash,1)+' HASHED'
           ELSE ' FROM WINDOWS'
      END + ';/*' + SP.type_desc + '*/ END;'
      COLLATE SQL_Latin1_General_CP1_CI_AS
 FROM sys.server_principals AS SP
 LEFT JOIN sys.sql_logins AS SL
   ON SP.principal_id = SL.principal_id
WHERE SP.type_desc IN ('SQL_LOGIN','WINDOWS_GROUP','WINDOWS_LOGIN')
  AND SP.name NOT LIKE '##%##'
  AND SP.name NOT IN ('SA');
```


•	As a double check a Per Database Option can be used
o	Apply Backup Restore Migration Steps – LoginsScript.SQL
o	Configure and Run BackupRestore.ps1 for Permissions Only

## **Copy Linked Servers**
•	Until an automated solution is documented, script out linked servers in SSMS and create them on the new server
•	Because passwords do not carry over, the password will need to be applied from the Password Manager wallet

## **Copy SQL Agent Jobs**
•	Script out SQL Agent Jobs From SSMS and Apply to New Server
-by Curt Coker

----------

A ROLLBACK of a named transaction initiated by a named BEGIN TRANSACTION will fail with an error.  The correct way to ROLLBACK only the work done in a called procedure is to do a SAVE TRANSACTION instead of a BEGIN TRANSACTION if the calling process already started a transaction.  If a SAVE TRANSACTION is used instead of BEGIN TRANSACTION, then a COMMIT should not be performed, since a COMMIT will COMMIT the work of the calling procedure.

In the following example, ROLLBACK TRANSACTION [Tran1] will fail because the [Tran1] transaction does not exist:

<PRE>
BEGIN TRANSACTION [Tran1] ;
BEGIN TRY
    UPDATE User SET IsDeletedFlag = 1 WHERE UserID = @ID ;
    UPDATE Contact SET IsDeletedFlag = 1 WHERE ContactID = @ID ;
    COMMIT TRANSACTION [Tran1] ;
END TRY
BEGIN CATCH
    ROLLBACK TRANSACTION [Tran1] ;
END CATCH
</PRE>

- ROLLBACK TRANSACTION [Tran1] fails with this kind of error following a BEGIN TRANSACTION [Tran1] command:
Msg 6401, Level 16, State 1, Line 24
Cannot roll back Tran1. No transaction or savepoint of that name was found.
- ROLLBACK TRANSACTION [Tran1] succeeds following a SAVE TRANSACTION [Tran1] command.
- ROLLBACK TRANSACTION (without a transaction or savepoint name) will roll back all open transactions, leaving nothing for a calling process to ROLLBACK if there is a problem.
- COMMIT TRANSACTION and COMMIT TRANSACTION [Tran1] will commit only the “inner” transaction (i.e. the most recently created transaction).  Since a SAVE TRANSACTION does not create a new transaction, either command following a SAVE TRANSACTION will COMMIT an existing “outer” transaction.  So, if a SAVE TRANSACTION was used instead of a BEGIN TRANSACTION, then there should be no COMMIT.

Here is a corrected version of the same code:

<PRE>
DECLARE @StartingTranCount int ;
--Note:  @@TRANCOUNT returns the number of nested transactions in the current session (NOT other sessions)
SET @StartingTranCount = @@TRANCOUNT ;  

IF @StartingTranCount = 0
    BEGIN TRANSACTION ;
ELSE
    SAVE TRANSACTION [uspMyProcedureName] ;  --Don’t use [Tran1], since that name could be re-used!!!

BEGIN TRY
    UPDATE User SET IsDeletedFlag = 1 WHERE UserID = @ID ;
    UPDATE Contact SET IsDeletedFlag = 1 WHERE ContactID = @ID     ;
    IF @StartingTranCount = 0
        COMMIT TRANSACTION ;
END TRY
BEGIN CATCH
    IF @StartingTranCount = 0
        ROLLBACK TRANSACTION ;
    ELSE
        ROLLBACK TRANSACTION [uspMyProcedureName] ;
    THROW;
END CATCH ;
</PRE>

Here is a relevant StackOverflow question and answer:  [https://stackoverflow.com/questions/9713350/save-transaction-vs-begin-transaction-sql-server-how-to-nest-transactions-nice](https://stackoverflow.com/questions/9713350/save-transaction-vs-begin-transaction-sql-server-how-to-nest-transactions-nice)



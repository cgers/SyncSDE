# SyncSDE
Re-synch SQL Server logins or users after restoring a database from backup and is based on the following
[ESRI Article](https://support.esri.com/en-us/knowledge-base/how-to-resynch-sql-server-logins-or-users-after-restori-000008079 "Re-synch SQL Server logins or users after restoring a database from backup") source. I am including a copy of this here as I frequently use this script and most recently when I searched for the article it took a long time to open and I was concerned that in the future this information may be archived.

# Summary
Instructions provided describe how to re-synch SQL Server logins with the database users after restoring a database from backup. This process is always required when an SDE schema database is restored from a backup (.bak), or attached from a previously detached database (.mdf). In these scenarios, the SDE user within the geodatabase is not in synch with the login for that instance of SQL Server.

If the SDE login is not added and synched with the user within that database, the connection fails with Bad Login User or the service does not start.

# Procedure
The stored procedure (SP_CHANGE_USERS_LOGIN) can be run to correct this problem by mapping an existing database user to their corresponding SQL Server login. This should be run against all databases in which the ‘SDE’ user requires access to manage the database. This also must be run for any SQL Server authenticated database users that own data. When starting the service, the ‘SDE’ user is the only user required.

1. Ensure the SQL Server login associated with the database user has been added to the instance under Security > Logins, prior to running the sp_change_users_login stored procedure.
2. Open a new query in SQL Server Management Studio and execute the following command:

```
use <database_name>
go
EXEC sp_change_users_login 'Update_One', 'sde', 'sde'
go
```

# The Reason
While working with databases in the context of Camunda BPM, we encounter some funny facts about databases and how they differ from each other.

## Identifiers
The max length of an identifier differs between dbs:
* Oracle: max of 30 characters for a table/index name
* Oracle: the maximum number of characters to store is restricted to 2000 (so ```Nvarchar2(2000)```) 
* MsSql: the ordering differs from the other databases. Let's assume we have the following entries: {P1:,P2:,P3:,P3:,P4:,P5:,P6:,P7:,P8:,P9:,P10:}. Then the shown ordering is also the one mssql returns. However, all other databases return the follwing ordering: {P10:,P1:,P2:,P3:,P3:,P4:,P5:,P6:,P7:,P8:,P9:}

## DB Specifics

### MySql/MariaDB

If a column of type `TIMESTAMP` is created without any constraint or with constraint `NOT NULL`, when an update statement is executed against the containing table, the value of this column's fields is automatically updated with the current timestamp. To prevent this, columns that need timestamp values should use the `DATETIME` type.

For compatibility with previous versions MySQL 5.6 and 5.7 `DATETIME` by default don't have fractional second precision. This differs from standard SQL and the other supported DB environments.
Docs: https://dev.mysql.com/doc/refman/5.7/en/fractional-seconds.html

### Oracle

People can configure their Oracle DB (starting from 19c) to drop manually created indexes if unused:
> You can use the AUTO_INDEX_RETENTION_FOR_MANUAL configuration setting to specify a period for retaining unused non-auto indexes (manually created indexes) in a database. The unused non-auto indexes are deleted after the specified retention period.

Docs: https://docs.oracle.com/en/database/oracle/oracle-database/19/admin/managing-indexes.html#GUID-A8B4BB05-2711-497A-8276-127076DAA518

Oracle treats strings with zero length (i.e. `""`) as `null`. This becomes relevant when empty strings are stored in the database and not null-checked upon retrieval. (See https://jira.camunda.com/browse/CAM-11448)

Docs: https://docs.oracle.com/database/121/SQLRF/sql_elements005.htm

### MSSQL

By default the collation is case insensitive.
That can lead to OptimisticLockingException if two variables "Count" and "count" are set in the same process for example

Docs: https://docs.microsoft.com/en-us/sql/relational-databases/collations/set-or-change-the-server-collation?view=sql-server-ver15#setting-the-server-collation-in-managed-instance

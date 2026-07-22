# SQL Server

## Links

* [Using Hints To Test SQL Server Indexes](https://www.mssqltips.com/sqlservertip/2045/using-hints-to-test-sql-server-indexes/)
* [SQL Fiddle - Online SQL syntax validator](http://sqlfiddle.com/)
* [ApexSQL Refactor - SQL Formatter AddIn for SSMS](http://www.apexsql.com/zips/ApexSQLRefactor.exe)
* [DB Comparer - Schema comparison tool](http://dbcomparer.com/)
* [Idera SQL Check - Connection/transaction monitor](https://www.idera.com/productssolutions/freetools/sqlcheck)
* [Transact-SQL (T-SQL) reference](https://learn.microsoft.com/en-us/sql/t-sql/language-reference)
* [SQL Server technical documentation](https://learn.microsoft.com/en-us/sql/sql-server/)
* [Back up and restore SQL Server databases](https://learn.microsoft.com/en-us/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases)
* [Dynamic Management Views and Functions (DMVs)](https://learn.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views)

## Restore History

```sql
SELECT * FROM msdb.dbo.restorehistory ORDER BY 2 DESC
```

## Rename a Database

```sql
USE master
ALTER DATABASE Compras_Bolivia SET SINGLE_USER WITH ROLLBACK IMMEDIATE
ALTER DATABASE Compras_Bolivia MODIFY NAME = [BO_COMPRAS]
ALTER DATABASE BO_COMPRAS SET MULTI_USER
```

## Find Columns by Name

```sql
SELECT sys.columns.name column_name, sys.tables.name table_name
FROM sys.columns
INNER JOIN sys.tables
    ON sys.columns.object_id = sys.tables.object_id
WHERE sys.columns.name LIKE '%column_name_pattern%'
```

## Search Text Across All Tables

```sql
USE [Database]

DECLARE @SearchStr VARCHAR(MAX) = 'search_term'
DECLARE @Results TABLE (columnname NVARCHAR(370), columnvalue NVARCHAR(3630))
DECLARE @TableName NVARCHAR(256), @ColumnName NVARCHAR(128)
DECLARE @SearchStr2 NVARCHAR(110) = CHAR(39) + '%' + @SearchStr + '%' + CHAR(39)

SET NOCOUNT ON
SET @TableName = ''

WHILE @TableName IS NOT NULL
BEGIN
    SET @ColumnName = ''
    SET @TableName = (
        SELECT MIN(QUOTENAME(table_schema) + '.' + QUOTENAME(table_name))
        FROM information_schema.tables
        WHERE table_type = 'BASE TABLE'
          AND QUOTENAME(table_schema) + '.' + QUOTENAME(table_name) > @TableName
          AND OBJECTPROPERTY(OBJECT_ID(QUOTENAME(table_schema) + '.' + QUOTENAME(table_name)), 'IsMSShipped') = 0
    )

    WHILE (@TableName IS NOT NULL) AND (@ColumnName IS NOT NULL)
    BEGIN
        SET @ColumnName = (
            SELECT MIN(QUOTENAME(column_name))
            FROM information_schema.columns
            WHERE table_schema = PARSENAME(@TableName, 2)
              AND table_name = PARSENAME(@TableName, 1)
              AND data_type IN ('char', 'varchar', 'nchar', 'nvarchar')
              AND QUOTENAME(column_name) > @ColumnName
        )

        IF @ColumnName IS NOT NULL
        BEGIN
            DECLARE @query VARCHAR(MAX)
            SELECT @query = 'SELECT ' + CHAR(39) + @TableName + '.' + @ColumnName + CHAR(39) +
                            ', LEFT(' + @ColumnName + ', 3630) FROM ' + @TableName +
                            ' (NOLOCK) WHERE ' + @ColumnName + ' LIKE ' + @SearchStr2
            INSERT INTO @Results EXEC(@query)
        END
    END
END

SELECT columnname, columnvalue FROM @Results
```

## Search Text in Stored Procedures

```sql
SELECT DISTINCT o.name AS Object_Name, o.type_desc, m.definition
FROM sys.sql_modules m
INNER JOIN sys.objects o ON m.object_id = o.object_id
WHERE m.definition LIKE '%search_text%'
```

## Search Text in Stored Procedures Across All Databases

```sql
DECLARE @DatabaseName AS NVARCHAR(128)
DECLARE @SQL AS NVARCHAR(500)
DECLARE @RESULTS TABLE (
    DatabaseName NVARCHAR(128),
    ObjectName   NVARCHAR(128),
    TypeDesc     NVARCHAR(60),
    Definition   NVARCHAR(4000)
)

DECLARE MY_CURSOR CURSOR LOCAL STATIC READ_ONLY FORWARD_ONLY
FOR SELECT name FROM sys.databases WHERE owner_sid <> 0x01

OPEN MY_CURSOR
FETCH NEXT FROM MY_CURSOR INTO @DatabaseName

WHILE @@FETCH_STATUS = 0
BEGIN
    SET @SQL = 'SELECT DISTINCT ''' + QUOTENAME(@DatabaseName) + ''' AS DatabaseName, ' +
               'o.name AS ObjectName, o.type_desc AS TypeDesc, m.definition AS Definition ' +
               'FROM ' + QUOTENAME(@DatabaseName) + '.sys.sql_modules m ' +
               'INNER JOIN ' + QUOTENAME(@DatabaseName) + '.sys.objects o ON m.object_id=o.object_id ' +
               'WHERE m.definition Like ''%search_text%'''

    INSERT INTO @RESULTS EXEC(@SQL)
    PRINT 'Checking Database ' + @DatabaseName
    FETCH NEXT FROM MY_CURSOR INTO @DatabaseName
END

CLOSE MY_CURSOR
DEALLOCATE MY_CURSOR

SELECT * FROM @RESULTS
```

## Monitoring Active Connections

```sql
sp_who2
```

## SQL Server 2000 DTS Designer in Management Studio

To use DTS Designer in SQL Server Management Studio 2008+, copy the following files from `%ProgramFiles%\Microsoft SQL Server\80\Tools\Binn` to `%ProgramFiles%\Microsoft SQL Server\100\Tools\Binn\VSShell\Common7\IDE`:

- `SEMSFC.DLL`
- `SQLGUI.DLL`
- `SQLSVC.DLL`

Also copy the corresponding `.RLL` files from `...\80\Tools\Binn\Resources` to `...\100\Tools\Binn\VSShell\Common7\IDE\Resources\<lang_id>` (e.g. `1033` for en-US).

## Generate C# Classes from JSON in Visual Studio

Copy the JSON (`Ctrl+C`), then in the `.cs` file go to:

Edit > Paste Special > Paste JSON as Classes

(Works in VS2012+ with .NET 3.5+)

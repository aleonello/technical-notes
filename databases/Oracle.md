# Oracle

## Links

* [Oracle Database SQL Language Reference](https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/index.html)
* [Oracle Data Pump Export and Import](https://docs.oracle.com/en/database/oracle/oracle-database/19/sutil/oracle-data-pump-overview.html)
* [Oracle Data Provider for .NET (ODP.NET) documentation](https://docs.oracle.com/en/database/oracle/oracle-database/19/odpnt/index.html)
* [Oracle Database Concepts](https://docs.oracle.com/en/database/oracle/oracle-database/19/cncpt/index.html)

## Count Objects by Type

Useful for comparing source/destination during a database update:

```sql
SELECT object_type, COUNT(object_name) num_objetos
FROM   user_objects
GROUP BY object_type
ORDER BY num_objetos
```

## Export

```sh
EXP <user>/<password>@<SID> FILE=<path>/EXPORT.DMP LOG=<path>/EXPORT.LOG STATISTICS=NONE
```

## Import

```sh
imp <user>/<password>@<SID> file=<export.dmp> log=log.log commit=Y fromuser=<source_user>
```

## Export Error: Invalid Identifier

```
EXP-00008: ORACLE error 904 encountered
ORA-00904: "POLTYP": invalid identifier
```

This means the Oracle client version is incompatible with the database version. Log directly into the database server to perform the export.

## Change Listener Port

```sql
EXEC DBMS_XDB.SETHTTPPORT(8181);
```

## Check User Permissions and TableSpace Before Deleting a User

In PL/SQL Developer: right-click on the username > View > "View SQL" button.

Example output to recreate the user:

```sql
-- Create the user
CREATE USER <username>
IDENTIFIED BY "<password>"
DEFAULT TABLESPACE <tablespace_name>
TEMPORARY TABLESPACE TEMP
PROFILE DEFAULT;

-- Grant/Revoke role privileges
GRANT CONNECT TO <username>;
GRANT DBA TO <username>;
GRANT RESOURCE TO <username>;

-- Grant/Revoke system privileges
GRANT UNLIMITED TABLESPACE TO <username>;
```

## Register ODP.NET Provider

```sh
OraProvCfg /action:config /product:odp /frameworkversion:v2.0.50727 /providerpath:C:\oraclexe\app\oracle\product\11.2.0\server\odp.net\bin\2.x\Oracle.DataAccess.dll
OraProvCfg /action:config /product:odp /frameworkversion:v4.0.30319 /providerpath:C:\oraclexe\app\oracle\product\11.2.0\server\odp.net\bin\4\Oracle.DataAccess.dll
OraProvCfg /action:gac /providerpath:C:\oraclexe\app\oracle\product\11.2.0\server\odp.net\bin\2.x\Oracle.DataAccess.dll
```

## Check and Set NLS Date Format

```sql
SELECT value FROM v$nls_parameters WHERE parameter = 'NLS_DATE_FORMAT';

ALTER SESSION SET nls_date_format = 'DD-MM-RR';

SELECT SYSDATE FROM dual;
```

## Check NLS Session Parameters

```sql
SELECT * FROM nls_session_parameters;
```

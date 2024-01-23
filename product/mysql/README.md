# MySQL

|             |             |
| ----------- | ----------- |
| Product     | **MySQL**   |
| Type        | Relational Database (RDBMS) |
| Website     | [https://wwww.mysql.com](https://www.mysql.com)|
| Created     | 2024-01-22  |


## Startup File Configuration

The default configuration file historically was `/etc/my.cnf` or `/etc/mysql/my.cnf`.
More recent versions and distributions may place these files across multiple directories.
This command attempts to capture the majority of default files and `.cnf` files found
in `/etc/mysql`.

```
(cat /etc/my.cnf 2>/dev/null; find /etc/mysql -name *.cnf -exec cat {} \;) | grep -v ^#
```

## Runtime Configuration

The preferred method available since MySQL 5.5 is to use the following SQL. This can be executed with the `mysql` client.

```
SELECT * FROM performance_schema.global_variables;
```
For more information see the [MySQL manual page](https://dev.mysql.com/doc/refman/8.0/en/performance-schema-system-variable-tables.html) for `performance_schema.global_variables`.

For current or earlier versions of MySQL run the following SQL with the `mysql` client.

```
SHOW GLOBAL VARIABLES;
```

For more information see the [MySQL manual page](https://dev.mysql.com/doc/refman/8.0/en/show-variables.html) for `SHOW VARIABLES`.

### Example Output

Depending on the version this output can contain 300+ variables.

```
+------------------------------------------------+------------------------------------------+
| Variable_name                                  | Value                                    |
+------------------------------------------------+------------------------------------------+
| activate_all_roles_on_login                    | OFF                                      |
| admin_address                                  |                                          |
| admin_port                                     | 33062                                    |
| admin_ssl_ca                                   |                                          |
| admin_ssl_capath                               |                                          |
| admin_ssl_cert                                 |                                          |
| admin_ssl_cipher                               |                                          |
| admin_ssl_crlpath                              |                                          |
| admin_ssl_key                                  |                                          |
| admin_tls_ciphersuites                         |                                          |
| admin_tls_version                              | TLSv1,TLSv1.1,TLSv1.2,TLSv1.3            |
| auto_generate_certs                            | ON                                       |
| auto_increment_increment                       | 1                                        |
```

## Variants

### AWS RDS

Individual MySQL configuration is held in a DB parameter group.

You can use this AWS CLI command to list all DB parameter groups:

```
$ aws rds describe-db-parameter-groups --no-cli-pager --output=text --query '*[].[DBParameterGroupName]'
```

You can then list the variables of the specific DB parameter group name with:

```
PG_NAME="default.mysql5.7"
aws rds describe-db-parameters --no-cli-pager --output=text --query '*[].[ParameterName,ParameterValue]' --db-parameter-group-name "${PG_NAME}"
```

NOTE: This output does not provide the MySQL version, you will need to determine this from the DB parameter group settings.

The full output of AWS RDS parameters is not required. It does contain additional attributes including:

```
{
    "ParameterName": "log_error_verbosity",
    "Description": "Controls verbosity of the server in writing error, warning, and note messages to the error log.",
    "Source": "engine-default",
    "ApplyType": "dynamic",
    "DataType": "integer",
    "AllowedValues": "1-3",
    "IsModifiable": true,
    "ApplyMethod": "pending-reboot"
},
```

### AWS RDS Aurora

Individual MySQL configuration is held in a DB cluster parameter group as well as one or more DB parameter groups.

You can use this AWS CLI command to list all DB cluster parameter groups:

```
$ aws rds describe-db-cluster-parameter-groups --no-cli-pager --output=text --query '*[].[DBClusterParameterGroupName]'
```

You can then list the variables of the specific DB parameter group name with:

```
CLUSTER_PG_NAME="default.mysql8.0"
aws rds describe-db-cluster-parameters --no-cli-pager --output=text --query '*[].[ParameterName,ParameterValue]' --db-cluster-parameter-group-name "${CLUSTER_PG_NAME}"
```

NOTE: This output does not provide the MySQL version, you will need to determine this from the DB cluster parameter group settings.

For individual DB parameter groups used by a DB cluster, see the prior section.

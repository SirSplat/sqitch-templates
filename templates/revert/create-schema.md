# Usage Documentation for Revert Template (`revert/create-schema.tmpl`)

This document provides detailed usage instructions for the **revert** template used with the `create-schema.tmpl` Sqitch change. The **revert** template is essential for rolling back a previously deployed schema change in PostgreSQL.

## Template Overview

The `revert` template is used to remove a schema from the PostgreSQL database. This operation is performed within a transaction to ensure that the rollback is atomic and consistent.

See PostgreSQL documentation for [DROP SCHEMA](https://www.postgresql.org/docs/current/sql-dropschema.html) implementation.

## Template Structure

```sql
/* Generated by: ~/.sqitch/templates/revert/create-schema.tmpl */

-- Revert [% project %]:[% change %] from [% engine %]

BEGIN;

DROP SCHEMA [% schemaname %];

COMMIT;
```

## Parameter Explanation

- **`[% schemaname %]`**: The name of the schema to be dropped. This parameter is taken from the options when running the `sqitch add` command, it is to ensure that the correct schema is removed.

## How It Works

- **BEGIN;** and **COMMIT;**: The `BEGIN;` and `COMMIT;` statements ensure that the `DROP SCHEMA` operation is executed within a transaction block. This guarantees atomicity—if the `DROP SCHEMA` statement fails, the transaction will be rolled back to preserve the database state.
- **DROP SCHEMA**: This command removes the specified schema from the database. Since Sqitch ensures that rollbacks are performed in the reverse order of deployment, the `revert` template will work as expected.

## Usage Scenarios

### Example 1: Basic Revert of a Schema (the last change deployed)

**Command**:
```bash
sqitch revert db --to @HEAD^
```

**Generated SQL**:
```sql
/* Generated by: ~/.sqitch/templates/revert/create-schema.tmpl */

-- Revert my_project:create_appschema from PostgreSQL

BEGIN;

DROP SCHEMA appschema;

COMMIT;
```

**Explanation**: This command reverts the `appschema` schema by dropping it from the database.

## Best Practices

- **Backup**: Always ensure you have a backup of your database or schema before running the `revert` operation.
- **Testing**: Test the `revert` operation in a development or staging environment before applying it to production.
- **Sqitch Order Assurance**: Rely on Sqitch's mechanism that only allows rollbacks to occur in reverse deployment order to ensure consistency.

## Conclusion

The **revert** template is an essential part of managing schema changes in a **Sqitch** project. It ensures that schema changes can be rolled back safely and consistently. Follow best practices to ensure a smooth rollback process and maintain database integrity.

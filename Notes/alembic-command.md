# Alembic command

## Generate a new migration script: Use the alembic revision --autogenerate command to generate a new migration script based on the changes you made to the model.

```bash
  alembic revision --autogenerate -m "Descriptive_migration_name"
```
## Apply the migration: To apply the migration and update the database schema, use the alembic upgrade command:

```bash
  alembic upgrade head
```

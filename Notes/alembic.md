# Alembic

#alembic #command #python

### Init new alembic in current directory

```bash
alimbic init
```

#### Write setup in **alembic.ini**
```ini
sqlalchemy.url = postbresql://%(DB_USER)s@%(DB_HOST)s:%(DB_PORT)s/%(DB_NAME)s
```
#### Write configuration in **migrations/env.py**
```python

config = context.config
section = config.config_ini_section
config.set_section_option(section, "DB_USER", DB_USER)
config.set_section_option(section, "DB_HOST", DB_HOST)
config.set_section_option(section, "DB_USER", DB_PORT)
config.set_section_option(section, "DB_USER", DB_USER)
config.set_section_option(section, "DB_USER", DB_USER)
```
### Command for migration

- Generate a new migration script:
    Use the `alembic revision --autogenerate` command to generate a new migration
    script based on the changes you made to the model.

```bash
  alembic revision --autogenerate -m "Descriptive_migration_name"
```

- Apply the migration:
    To apply the migration and update the database schema,
    use the `alembic upgrade` command:

```bash
  alembic upgrade head
```


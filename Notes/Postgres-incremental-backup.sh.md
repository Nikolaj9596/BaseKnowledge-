# Create increamental backup for postgresql


### docker-compose.yaml
```yaml
  db:
    image: postgres:12.4
    container_name: postgres
    volumes:
      - ./postgresql/data:/var/lib/postgresql/data/
      - ./postgresql/postgresql.conf:/var/lib/postgresql/data/postgresql.conf
      - ./postgresql/wal_file:/home/backup
      - ./postgresql/incremental_backup:/home/incremental_backup
    environment:
      POSTGRES_PASSWORD: giveaway_api
      POSTGRES_USER: giveaway_api
      POSTGRES_DB: giveaway_api
    restart: always
    env_file:
      - ./.env
    ports:
      - 5432:5432

```

### postgresql.conf
```conf

wal_level = replica			# minimal, replica, or logical
archive_mode = on		# enables archiving; off, on, or always
# Not overwriting files is a good practice
archive_command = 'test ! -f /home/backup/%f && cp %p /home/backup/%f'
archive_timeout = 300	# force a logfile segment switch after this
# These are only used in recovery mode.
restore_command = 'cp /home/backup/%f %p'
recovery_target_timeline = 'latest'	# 'current', 'latest', or timeline ID
```
### Enter command to container postgresql
```bash
chown -R postgres:postgres /home/incremental_backup
chown -R postgres:postgres /home/backup
pg_basebackup -D /home/incremental_backup/ -Ft -Xfetch -z -P -U $POSTGRES_USER
sudo tar -xf postgresql/incremental_backup/base.tar.gz -C postgresql/data   
```

## For restore

1. Stop postgresql
2. Remove old data
3. Untar base backup
4. Copy data from backup to postgresql
5. Create file for send siganl server restore use wal files
6. Start postgresql

```bash
docker-compose down
sudo rm -rf postgresql/data
mkdir postgresql/data
sudo tar -xf postgresql/incremental_backup/base.tar.gz -C postgresql/data
sudo echo restore_command = 'cp /home/backup/%f "%p"' > postgresql/data/recovery.signal 
docker-compose up -d
```

Postgres Backup
=========

This role creates the scripts, and cronjobs necessary to backup all your postgres databases.
The scripts are copied from: https://wiki.postgresql.org/wiki/Automated_Backup_on_Linux

Requirements
------------

- Working postgres server
- Access to all databases for the user postgres

Role Variables
--------------
```yaml
---
# wether to rotate/delete expired logs
postgres_backup_rotate: True

# Where to place the backup config and script
postgres_backup_script_location: /etc/postgres/backup-script

postgres_backup_cronjob_enabled: True
postgres_backup_cronjob_schedule: "0 3 * * *"

# The following configurations are all mapped to values in the config file

# defaults file for ansible-role-postgres-backup
# If the user the script is running as doesn't match this
# the script terminates.  Leave blank to skip check.
# This is also be used as the owner of the script, and the user into which crontab we enter the cronjob
postgres_backup_script_user: "postgres"

# hostname to adhere to pg_hba policies.  Will default to "localhost" if none specified.
postgres_backup_db_hostname: ""

# This dir will be created if it doesn't exist.  This must be writable by the user the script is
# running as.
postgres_backup_directory: "/mnt/backup/"

# List of strings to match against in database name, for which we only wish to keep a backup of 
# the schema, not the data. Any database names which contain any of these values will be 
# considered candidates. (e.g. "system_log" will match "dev_system_log_2010-01")
# Will be joined by commas
postgres_backup_schema_only: []

# Will produce a custom-format backup if set to True
postgres_backup_custom: True

# Will produce a gzipped plain-format backup if set to True
postgres_backup_plain: True

# Will produce gzipped sql file containing the cluster globals, like users and passwords, if set to True
postgres_backup_globals: True

# Which day to take the weekly backup from (1-7 = Monday-Sunday)
# Only relevant if postgres_backup_rotate = true
postgres_backup_day_of_week: 5

# Number of days to keep daily backups
# Only relevant if postgres_backup_rotate = true
postgres_backup_retention_days: 7

# How many weeks to keep weekly backups
# Only relevant if postgres_backup_rotate = true
postgres_backup_retention_weeks: 5
```

Dependencies
------------

`pg_dump` must exist on the host that the script is installed to  
To enable the cronjob, `cron` must be installed as well.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: postgres_servers
      roles:
         - { role: peschmae.ansible-role-postgres-backup }

License
-------

MIT

Author Information
------------------

dev@peschmae.ch

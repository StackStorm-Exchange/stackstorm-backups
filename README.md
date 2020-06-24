# Backups Pack

This pack backs up the MongoDB and PostgreSQL databases in a StackStorm deployment. 
It is currently best used on a single node deployment, but has the potential to be used
for HA setups.

## Pre-requisites

The `st2actionrunner` node(s) that executes the actions must have the following system-level
packages installed:

- `mongodb-org-tools`  (for the `mongodump` tool)
- `postgresql` (for the `pg_dump` tool)

# Configuration

This pack's configuration is done via the datastore, not config file.
The following keys need to be set:

| Scope  | Key | Secret | Description |
|--------|-----|--------|-------------|
| system | backup.retention_days  | false | Passed into the `find` command, deletes files based on this many days. To find files older than n days you should specify `+n` . If you specify just `n` it will find files that are exactly `n` days old. |
| system | mongodb.admin_password | true  | Password of the `admin` user for MongoDB |
| system | mongodb.admin_username | false | Username of the `admin` user for MongoDB |
| system | mongodb.mongodb_host   | false | Hostname of the MongDB system to backup |

## Example

```shell
# save backups for 2 weeks
st2 key set backup.retention_days "+14"

# set our mongodb username
st2 key set mongodb.admin_username "admin"

# set our mongodb password
st2 key set -e mongodb.admin_password "Secret!"

# set our mongodb host
st2 key set mongodb.mongodb_host "server01"
```

# Actions

The pack provides a few simple actions for backing up the databases that StackStorm
is built on:

| Action            | Description |
|-------------------|-------------|
| `full_backup`     | Performs a backup of both MongoDB and PostgreSQL databases |
| `mongodb_backup`  | Performs a backup of the StackStorm MongoDB database       |
| `postgres_backup` | Performs a backup of the StackStorm PostgreSQL database    |

## Example

```shell
# perform a full backup
st2 run backups.full_backup

# perform backup of mongodb
st2 run backups.mongodb_backup

# perform backup of postgres
st2 run backups.postgres_backup
```

# Rules

By default we do not ship with a rule to enable backups because every organization
and deployment will have its own schedule. Instead we suggest that you create
a rule that invokes the backup actions within a pack that your organization deploys.

Here is an example rule that can be used to schedule backups on cron trigger:

```yaml
---
name: full_backup_cron
pack: mypack
description: "Executes a full backup on a cron schedule."
enabled: true

trigger:
  type: "core.st2.CronTimer"
  # http://apscheduler.readthedocs.io/en/3.0/modules/triggers/cron.html#api
  parameters:
      timezone: "UTC"
      day_of_week: "*"
      hour: 1
      minute: 0
      second: 0
  
action:
  ref: "backups.full_backup"

```

# TODO

- Allow passwords and usernames to be set via config instead of being in the key/value store
- Allow PostgreSQL password to be passed in via parameters
- Allow MongoDB username/password to be read in from the /etc/st2/st2.conf
- Implement backups of PostgreSQL database (allow all credentials/configs to be passed in with
 parameters)
- Investigate using Orchestra or ActionChains instead of Mistral

# backups Integration Pack

## Configuration
TODO: Describe configuration


# Sensors

## Example Sensor
TODO: Describe sensor


# Actions

## example
TODO: Describe action

# Rules

By default we do not ship with a rule to enable backups because every organization
and deployment may need a different schedule. Instead we suggest that you create
a rule that invokes the backup actions within a pack that your organization deploys.

Here is an example rule that can be used to schedule backups on cron trigger:

``` yaml
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

- Implement backups of remote databases (allow all credentials/configs to be passed in with parameters)

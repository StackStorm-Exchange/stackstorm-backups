# Change Log

## v1.0.1

* Added backup.timeout key value look up for backup_timeout param
* Fixed issue with full_backup workflow that if only 1 of the methods failed
  it would still exit successful.

  Contributed by Bradley Bishop (@nmaludy Encore Technologies)

## v1.0.0

* Migrated from Mistral -> Orquesta (Feature)

  Contributed by Nick Maludy (@nmaludy Encore Technologies)

* Added compression-in-place instead of dumping to a file/directory then running gzip.
  This reduces the required disk space to execute backups, because we don't have to store
  the intermediate uncompress data. (Feature)

  Contributed by Nick Maludy (@nmaludy Encore Technologies)

* Added `--quiet` to the `mongodump` command, so that progress bars don't litter the `stdout`
  of the action execution. (Bugfix)

  Contributed by Nick Maludy (@nmaludy Encore Technologies)

## v0.1.2

Added `backup_timeout` parameter to all actions to control the timeout of the long-running
backup commands within the workflows (`mongodump`, `pg_dump`, `tar`, etc).

## v0.1.1

Added pack icon. No code changes.

## v0.1.0

Initial Revision

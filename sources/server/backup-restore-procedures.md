# Backup and Restore procedures for Shippable Server

Regular backups should be part of your Server management checklist. This page shares our recommendations for backing up essential pieces of Shippable Server so that you do not lose data in case your data is corrupted or you need to reinstall Server for any reason.

##Frequency of backups

We recommend you backup the components mentioned once every 24 hours.


## Backup instructions

You should backup the following components:

* [Postgres database](#backup-Postgres)
* [Gitlab](#backup-Gitlab)
* [Installer state.json](#backup-installer)  
* [Decryption keys for Vault](#backup-vaultKeys)

Our recommended approach is to mount a special drive for the machine where the backups are taken, and copy backups to this drive. If something bad happens to the machine, you can bring up a new machine, attach the drive, and restore backups.

Please note that Redis does not need to be backed up since it is only used to cache user sessions. A simple reload of the page should restore state.

RabbitMQ backup and restore procedure is provided, but is not strictly necessary. If an admin of Shippable Server signs in and 'Force Sync's their account, all queues are automatically restored. However, to avoid any potential problems with this, it is safer to back up RabbitMQ as well.

<a name="backup-Postgres"></a>
### Backup/Restore for Postgres database

To backup your database, follow the steps below:

* ssh into your Core machine which has the Postgres DB
* Run the following commands (replace <backup_filename> with your desired backup file name):

```
$ mkdir /backup

$ sudo chown postgres:postgres /backup

$ sudo su postgres

$ pg_dump shipdb > /backup/<backup_filename>.sql

```

* Copy the file to your mounted external drive or to whatever location you want to store your backups at.

To restore your Postgres database, follow the steps below:

* If your Core machine is being re-provisioned, follow the following 3 steps. If not, then skip to the next step:
    * Update `base/usr/machine.json` on your Swarm master with the IP address of the new machine
    * Update `/base/usr/state.json` on Swarm Master and change the `databaseInstalled`, `databaseInitialized` flags to false.
    * Run the installer again. This will bootstrap the database.
* Recover Postgres from the latest sql file with the following commands on the db machine:

```
$ sudo su postgres
$ psql shipdb < <backup_filename>.sql

```

Please note that if your Core machine was re-provisioned, you will also need to restore Vault and RabbitMQ.

<a name="backup-Gitlab"></a>
### Backup/Restore for GitLab

The backup procedure for Gitlab involved backing up the configuration and then the application. To do this:

* ssh into the Swarm Master or whichever machine has Gitlab installed
* Copy `/etc/gitlab` to your external drive or any location of your choice. This contains configuration and keys. This does not change frequently and does not necessarily need to be backed up every day.
* Run `sudo gitlab-rake gitlab:backup:create`. This will create a backup in `/var/opt/gitlab/backups`. Copy this file to your external drive or any location of your choice. You may notice some elements are marked [SKIPPED] in the console when you run this command. This is intended behaviour -- repositories with no content are not backed up because there is nothing to backup.

To restore Gitlab:

* If Gitlab needs to be reinstalled, update `/base/usr/state.json` on Swarm Master and change the gitlabInitialized and gitlabInstalled flags to false.
* Run the installer and a new instance of Gitlab will be installed.
* Restore the configuration files to `/etc/github` and run `sudo gitlab-ctl reconfigure`
* Ensure the latest application backup file is placed in the `/var/opt/gitlab/backups/` directory
* Run the following commands to stop all gitlab processes that are connected to the database:

```
$ sudo gitlab-ctl stop unicorn
$ sudo gitlab-ctl stop sidekiq
# Verify
$ sudo gitlab-ctl status
$ sudo gitlab-rake gitlab:backup:restore BACKUP={timestamp}

```

In the last command above, {timestamp} is the first part of your backup's file name (e.g., if your backup file is named 1481302306_gitlab_backup.tar, use 1481302306 as the timestamp for the restore command)

* Restart Gitlab

<a name="backup-vaultKeys"></a>
### Backup/Restore for Vault keys

All Vault data is backed up and restored with Postgres. However, keys need to be backed up and restored separately since Vault needs the ability to decrypt data stored in Postgres.

To backup Vault keys:

* ssh into your Swarm Master machine
* Copy `/etc/vault.d/keys.txt` to your external drive or to a secure location of your choice.

To restore Vault keys:

* Make sure the database recover is already performed.
* ssh into your Core machine. Vault is installed there.
* Create a `/etc/vault.d` folder with 755 permissions
* Copy the backed up `keys.txt` to `/etc/vault.d` folder
* On the Swarm machine, update `/base/usr/state.json` and change `vaultInstalled` & `vaultInitialized` in installStatus to `false`  
* Run the installer

<a name="backup-installer"></a>
### Backup/Restore for Installer state.json

The installer is always aware of the latest state of your Server installation. This state is stored on Swarm Master at `/base/usr/state.json`.

To ensure that you always have the latest state even if the Swarm Master is re-provisioned due to any reason, copy `/base/usr/state.json` to the external drive or a secure location of your choice as part of the backup procedures.

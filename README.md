revenni.restic
=========

Ansible role providing restic functionality for backups to Backblaze B2.

[![Platforms](http://img.shields.io/badge/platforms-ubuntu-lightgrey.svg?style=flat)](#)
[![Platforms](https://img.shields.io/badge/x64-64--bit-blue)](#)
[![Licence](https://img.shields.io/badge/Licence-MIT-blue.svg)](https://tldrlegal.com/license/mit-license)

Requirements
------------

1. This role makes use of anisble-vault to store sensitive data. You can use inline vault strings for the variables noted as sensitive below.

Make sure you don't lose the passphrase otherwise you will not be able to run the playbook until you recreate the vault.

Role Variables
--------------

### Sensitive Variables (declared in vars/vault.yml)
* ```vault_restic_account_id``` ('') - account id string from b2
* ```vault_restic_account_key``` ('') - account key string from b2
* ```vault_restic_encryption_key``` ('') - encryption key for data stored in cloud

To generate encrypted strings for these variables run the following:

* ```ansible-vault encrypt_string 'account_id' --name 'vault_restic_account_id'```
* ```ansible-vault encrypt_string 'account_key' --name 'vault_restic_account_key'```
* ```ansible-vault encrypt_string 'encryption_key' --name 'vault_restic_encryption_key'```

### Generic variables (declared in defaults/main.yml + overridden in vars/main.yml)
* ```restic_cron_email``` (your@email.address) used for cron notifications
* ```restic_path``` (/opt/restic)  - path to install restic scripts
* ```restic_log_file``` (/var/log/restic.log)  - path to install restic scripts
* ```restic_repository``` ('b2:Revenni-Bucket:{{ ansible_fqdn }}') - b2:bucket:directory
* ```restic_cron_schedule``` (0 */12 * * *) - crontab(5) style schedule
* ```restic_retention``` (30d) - number of days to store snapshots

### Optional variables (declared in vars/main.yml)
We backup / by default not traversing mountpoints.  If you have additional mount points you would like backed up they can be specified in the following variable.

* ```restic_additional_mounts``` (string) space delimited string of additional mounts to backup

Dependencies
------------

* None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      become: true
      roles:
         - { role: revenni.restic, tags: restic }

License
-------

MIT

Author Information
------------------
* [Vince Hillier](https://revenni.com) | [e-mail](mailto:vince@revenni.com) | [Twitter](https://twitter.com/vincedotca)

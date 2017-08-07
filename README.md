# Sugar Labs DbConfigPasswordCrypt
This project customises the system so that the config.php database password for Sugar can be encrypted.<br/><br/>
The implementation has been completed by extending the Sugar db managers for MYSQLI and ORACLE.<br/>
Encryption is completed with blowfish encryption - variable-length key between 32 and 448 bits (between 4 and 56 characters key size).<br/>
The encryption key can either be stored on an environment variable as preferred option, and as a fallback the key will be auto-generated by the system within the blowfish folder.

NOTE: This project has been developed on Sugar 7.8.0.0 using a LAMP stack. Latst update completed on Sugar 7.9.1.0 using a LAMP stack.

## Installation

1. Set the encryption key on the environment variable and verify that it can be read correctly by Apache and by CRON. This step is required for environment variable encryption. It might be optional if the implementation can rely on file system encryption key storage (see below for more details)
2. Download the latest .zip package here: https://github.com/sugarcrmlabs/DbConfigPasswordCrypt/releases/latest
3. Load the package in your target instance using Module Loader
4. Update the `db_manager` config option on `config.php` to the matching custom db manager based on the db used (see below for more details)
5. Follow additional instructions output on Sugar's log. Instructions require to modify the database password variable with the encrypted blowfish password that is displayed on the system's instructions (see below for more details)

### Installation details

#### Environment variable
The environment variable for the encryption key is `sugar_blowfish_key`. It must be between 4 and 56 characters key size and it must be readable by the all systems/users that access the database via the application (eg: web servers, api only servers, cron server).

#### Db Managers
The `db_manager`'s `config.php` config option, needs to be modified either to `CustomMysqliManager` or `CustomOracleManager` depending on the `db_manager` used (eg: `$sugar_config['dbconfig']['db_manager'] = 'CustomMysqliManager';`).

#### Additional instructions
Additional instructions about encryption are shown on Sugar's log, similar to the ones displayed below:

```
********** IMPORTANT ENCRYPTION INSTRUCTIONS - START **********

Your database password is currently NOT encrypted. Please follow the two steps below to set up the encrypted password:

STEP 1: Change value of variable $sugar_config['dbconfig']['db_password'] = '<encrypted password here>'; on config.php
STEP 2: Append $sugar_config['dbconfig']['use_encryption'] = true; to file config_override.php

********** IMPORTANT ENCRYPTION INSTRUCTIONS - END **********
```

#### Prevent continuous security logging when using file system key storage
If step 1 of the installation process has not been completed (and therefore the encryption key will be automatically generated within the blowfish folder) you might want to follow the additional steps shown on Sugar's log, to inhibit the security log warning that the key is stored in the filesystem, by using the `config_override.php` option below:

`$sugar_config['dbconfig']['filesystem_encryption_only'] = true;`

### Change password

To change the database password, the Sugar configuration options will have to be reverted temporarily (`$sugar_config['dbconfig']['use_encryption'] = false;`) and the new database password needs to be input temporarily in clear text on config.php on `db_password`, to obtain the encrypted version on Sugar's log. Finally set again the `use_encryption` to true and the clear text password to the encrypted one.

## Contributing
Everyone is welcome to contribute to this project! If you make a contribution, then the [Contributor Terms](CONTRIBUTOR_TERMS.pdf) apply to your submission.

Please check out our [Contribution Guidelines](CONTRIBUTING.md) for helpful hints and tips that will make it easier for us to accept your pull requests.

-----
Copyright (c) 2017 SugarCRM Inc. Licensed by SugarCRM under the Apache 2.0 license.

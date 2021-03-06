---
title: Bakery Overview
metadata:
    description: An overview of the commands available in Bakery.
taxonomy:
    category: docs
---

UserFrosting's CLI, or [*Command-line Interface*](https://en.wikipedia.org/wiki/Command-line_interface), is called the **Bakery**. It provides a number of helpful commands that can assist you while you build, manage and install your application. To view a list of all available Bakery commands, you may use the `list` command from your UserFrosting root directory :

```bash
$ php bakery list
``` 

Every command also includes a "help" screen which displays and describes the command's available arguments and options. To view a help screen, simply precede the name of the command with help:

```bash
$ php bakery help [command]
``` 

General help can also be displayed by running:

```bash
$ php bakery help
``` 

## Available commands

### bake

Bake is the general installation command. It combines `setup`, `debug`, `migrate` and `build-assets` into a single command : 

```bash
$ php bakery bake
``` 

>>>>>> This command should be executed every time you run `composer update`, change assets, create a new sprinkle or install a [community sprinkle](/sprinkles/community).

### debug

The `debug` command will run a series of tests to make sure everything is ready to run UserFrosting on your system. If you have trouble accessing your UserFrosting installation, you should run this command first to make sure basic requirements are met. 

The information displayed by this command can also be useful to other people when [asking for help](/troubleshooting/getting-help) and submitting new issues on Github. 

```bash
$ php bakery debug
``` 

### setup

The `setup` command can be used to setup the database and email configuration. This can also be done manually by editing the `app/.env` file or using global server environment variables. See [Environment Variables](/configuration/environment-vars) for more information about these variables.

```bash
$ php bakery setup 
``` 

| Option      | Description                                    |  
|-------------|------------------------------------------------|
| -f, --force | If `.env` file exist, force setup to run again |

### build-assets

The `build-assets` command is an alias for the node.js and npm scripts used for asset management. The `/build` directory contains the scripts and configuration files required to download Javascript, CSS, and other assets used by UserFrosting. This command will install Gulp, Bower and other required npm packages locally. With npm set up with all of its required packages, it can be used to automatically download and install the assets in the correct directories.

See the [Asset Management](/asset-management) chapter for more information about asset bundles and the `compile` option.

```bash
$ php bakery build-assets
``` 
  
| Option        | Description                                                      |
|---------------|------------------------------------------------------------------|
| -c, --compile | Compile the assets and asset bundles for production environment  |
| -f, --force   | Force fresh install by deleting cached data and installed assets |

>>>>> The compile option is automatically added when the [environment mode](/configuration/config-files#EnvironmentModes) is set to `production`.

### migrate

>>>> Database migrations have the potential to destroy data.  **Always** back up production databases, and databases with important data, before running migrations on them.

The `migrate` command runs all the pending [database migrations](/database/migrations). Migrations consist of special PHP classes used to manipulate the database structure and data, creating new tables or modifying existing ones. UserFrosting comes with a handful of migrations to create the [default tables](/database/default-tables) and the master user. The built-in migrations also handle the changes in the database between versions. See the [Migrations](/database/migrations) section for more information about migrations.

```bash
$ php bakery migrate
``` 

| Option              | Description                              |
|---------------------|------------------------------------------|
| -p, --pretend       | Run migrations in "dry run" mode         |


The `pretend` option can be used to test migrations. Use `-vvv` to also display the underlying SQL queries:

```bash
$ php bakery migrate --pretend -vvv
```

### migrate:rollback

The `migrate:rollback` command allows you to cancel, or rollback, the last migration operation. For example, if something went wrong with the last migration operation or if you made a mistake in your migration definition, you can use that command to undo it. 

Note that migrations are run in batches. For example, when running the `migrate` command, if 4 classes (or migration definitions) are executed, all 4 definitions will be reverted when rolling back the last migration operation. 

Options can also be used to rollback more than one migration at a time or to rollback migrations from a specific sprinkle. 

```bash
$ php bakery migrate:rollback
``` 

| Option              | Description                              |
|---------------------|------------------------------------------|
| -s, --steps=STEPS   | Number of steps to rollback [default: 1] |
| --sprinkle=SPRINKLE | The sprinkle to rollback [default: ""]   |
| -p, --pretend       | Run migrations in "dry run" mode         |

### migrate:reset

The `migrate:reset` command is the same as the _rollback_ command, but it will revert **every** migration. Without options, this is the same as wiping the database to a clean state. **_Use this command with caution!_**.

The `--sprinkle=` option can also be used to reset only migrations from a specific sprinkle. 


```bash
$ php bakery migrate:reset
``` 

| Option              | Description                              |
|---------------------|------------------------------------------|
| --sprinkle=SPRINKLE | The sprinkle to rollback [default: ""]   |
| -p, --pretend       | Run migrations in "dry run" mode         |

### migrate:refresh

The `migrate:refresh` command will rollback the last migration operation and execute it again. This is the same as executing `migrate:rollback` and then `migrate`.

```bash
$ php bakery migrate:refresh
``` 

| Option              | Description                              |
|---------------------|------------------------------------------|
| -s, --steps=STEPS   | Number of steps to rollback [default: 1] |
| --sprinkle=SPRINKLE | The sprinkle to rollback [default: ""]   |
| -p, --pretend       | Run migrations in "dry run" mode         |

### clear-cache

The `clear-cache` command takes care of deleting all the cached data. See [Chapter 16](/advanced/caching) for more information.

```bash
$ php bakery clear-cache
``` 

### test

The `test` command is used to execute [PHPUnit](https://phpunit.de/) tests. See the [Unit Testing](/advanced/unit-tests) section for more information.

```bash
$ php bakery test
```

>>>> UserFrosting's built-in integration tests use a temporary in-memory SQLite database.  For testing to run successfully, you must have the `php-sqlite3` package installed and enabled.  Alternatively, you can create a separate testing database and override the `test_integration` database settings in the `testing.php` [environment mode](/configuration/config-files).

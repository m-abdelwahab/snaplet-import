#  Snaplet import

This is a GitHub action that uses [Snaplet](snaplet.dev) to import anonymized data from a source database to a target database. It runs on a schedule to keep the target database schema up to date.

> **Warning**
>
> Note that this project is experimental and is still being tested. If you run into any issues, please open an issue on this repository.



## How to use
To get started, click on the `Use this template` button to create a new repository from this template. This action runs every day at midnight, but you can change this to whatever schedule works best for you. You can use [crontab.guru](https://crontab.guru) to create CRON expressions.

## Configuring environment variables

Go to the repository's settings and click on the `Secrets` tab. Then, add the following environment variables:
- `SOURCE_DATABASE_URL`: The URL of the database to import data from
- `TARGET_DATABASE_URL`: The URL of the database to import data to

![image](https://user-images.githubusercontent.com/27310414/219452618-580232bc-dc15-4df9-8f8c-a2c2b1707fe1.png)

That's it! You're all set.

## How the Project works

The first step is to import the schema from the source database to the target database. This is done by running `pg_dump` with the `--schema-only` flag. Next, snaplet introspects the target database and make transformation suggestions for columns that could contain personally identifiable information (PII). Finally, the action creates a snapshot with anonymized data from the source database and restores it into the target database.

This flow runs on a schedule to keep the target database schema up to date.


## Why Snaplet?

Snaplet offers two key features that make it a great fit for this project:
1. It automatically introspects the database and makes suggestions for columns that could contain PII. This means no need to maintain a config file to make sure that you are not.
2. Under the hood, Snaplet uses [Copy Cat](https://github.com/snaplet/copycat), which is a library that generates deterministic fake values. This means when a specific value is anonymized, it will always be anonymized to the same value. This means you can use the anonymized data for testing purposes.

## Project limitations

The first limitation is Job execution time - Each job in a workflow can run for up to 6 hours of execution time. This means that if you have a large database, this action will not work for you right now.
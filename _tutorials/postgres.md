---
title: Everyday PostgreSQL
category: Ruby Gems
excerpt: "PostgreSQL up and running."
date: 2015-01-14 00:00:00 -0800
tags: [postgres, postgresql, psql, rails]
---
osx 10.10.1   |  Jan 14, 2015

PostgreSQL is the recommend database server for many production web applications. Not to mention required by [Heroku].

## Common Commands and Usage

Command line tools to create, view, and maintain a local Postgres installation.

### Cli

Tools available to us from the Terminal.

Tools |
--- | ---
`createdb <dbname>` |
`dropdb <dbname>` |

### psql

The `psql` command gives us an additional set of tool for looking in more detail at the available Postgres databases. `psql` provides two contexts of interaction. The psql command and an interactive terminal. The command provides several tools
but mostly related to connecting a `psql` session to a given database. From outside the psql session arguments are issued with a leading `-` and are often mirrored by similar commands issued from within the a psql session starting with a leading `\`.

### psql cli

Commend | Description
--- | ---
`psql -d <dbname>` | enter a psql session in a given db.
`psql -l` | List available databases

### Interactive psql terminal session

Commend | Description
 --- | ---
`\d` | prints out tables
`\d <table_name>` |  prints out columns of the specified table.
`\l` | List available databases.

## Installing Postgres

Postgres can by installed and managed in several ways. Each installation requires a slightly different configuration and admin tool(s).

- The [Postgres.app] which has an independently developed but purpose built admin app called [PG Commander].
- Via [Homebrew] (My method of choice)
- The site install Packages.

### Starting the Postgres Server

Fallowing the install of Postgres you must start the Postgres server. This is true for all of the various installations of Postgres I know of. Not having this running is the source of many first time Postgres difficulties.

The caveats shown in the terminal after the Homebrew installation documents the commands to start and link to auto-start at login. I set mine up to start at login. This causes the Postgres to be available all the time.

Alternatively their is a gem called [`launchy`] which gives you a simple commands to start and stop the server. (`launchy start postgres`, `launchy stop postgres`). I do not cover this method any further in this article.

The best way I know of to confirm whether an instance of Postgres is running is to use the *Activity Monitor* located under `Application < Utilities < ActivityMonitor`.

### Install Via *Homebrew*

Using Homebrew from the command line to install Postgres is my first choice. I find this the most direct way to get up and running. Also this method seams to work particularly well for Rails applications.

Assuming you have [xcode], [cURL], and [Homebrew] installed.
run:

```bash
  brew install postgres
```

After it finishes READ the caveats!!!
The next few steps are lifted directly form the post install "caveats". If you there any differences between what I have here and the caveats __go with the caveats.__

What needs to be done fallowing the installation is to link the Postgres application *plist file* to the system LaunchAgent . To do this first to make LaunchAgent directory.
run:

```bash
mkdir -p ~/Library/LaunchAgents
```

Second we will create the link to LaunchAgent directory.
run:

```bash
ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents
```

This third command will start the server immediately. After this if you choose to have it start when you login it will be there for you with no further effort needed. (If your using Launchy you **don't** need to do this part.)
run:

```bash
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
```

With all that done run: `which psql`. Were looking for this `/usr/local/bin/psql` output which tells us that were using the homebrew version of Postgress.

If this is your first time installing Postgres using homebrew you need to create a database.
Run:

```bash
  initdb `whoami`
```

### Save a data set

```bash
pg_dump -O -F t <dbmane> > <dbname_dump>
```

```bash
Restore: Be in the directory with dump_file.
```

```bash
pg_restore -C -d <database_name_to_revive_data> <name_of_saved_data_set>
```

[PostgreSQL]: http://www.postgresql.org/
[psql]: http://www.postgresql.org/docs/9.4/static/app-psql.html
[Postgres install package]: http://www.postgresql.org/download/macosx/
[Postgres.app]: http://postgresapp.com/
[PG Commander]: https://eggerapps.at/pgcommander/
[launchy]: https://github.com/copiousfreetime/launchy
[xcode]: https://developer.apple.com/xcode/
[cURL]: http://curl.haxx.se/docs/manpage.html
[Heroku]: https://www.heroku.com/
[Homebrew]: http://brew.sh/

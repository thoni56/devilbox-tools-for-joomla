# Devilbox/Joomla tools

Devilbox allows you to easily create staging areas for one or many
websites. If you, like me, are using Joomla for many sites, Akeeba
backup is the goto solution not only for backups but for automatic
restores.

- Devilbox - a Docker based HTTPD/PHP/MYSQL... environment
- Akeeba Backup and Kickstart - easy website backup and automated restore

## Installation

Clone or download this repo and add it to your path.

## Prerequisites

You need

- Devilbox
- PHP
- Kickstart - download from akeeba.com
- a `devilbox` command script (which just cd's into the Devilbox installation and executes `shell.sh`)
- `lftp` - used by `download-backup` to fetch backups over SFTP (works against hosts that allow the SFTP subsystem but no interactive shell). Install with `brew install lftp` on macOS, or your distro's package manager on Linux. Authentication uses your `~/.ssh/config` and ssh agent — no separate credentials needed.

## Context

Having taken a backup of your site using Akeeba Backup you want to
create a copy of the site so that you can test out some changes, or
upgrades.

Given the scripts in this repo you can restore a site from a backup
using a single command.


## Setup

I keep all my Devilbox projects in a directory separate from the
Devilbox installation. Read the Devilbox documentation on how to do
that.

So here's an example structure

```
├─ devilbox-projects
├── portfolio-copy
├─── htdocs
├── app-copy
├─── htdocs
```

Devilbox stipulates that each directory under the designated devilbox
project directory becomes its own site and the files must be placed in
an `htdocs` sub-directory.

I also use `localhost.tv` as the domain (which always points back to
`localhost`) so the local copies of sites will have URLs like
`http://portfolio-copy.localhost.tv`. Again read the Devilbox docs to
find out how to do that.


## Procedure

- Take a backup of your Joomla site using Akeeba backup
- Go to the local project directory (not the `htdocs`)

Either
- Download the backup file(s) to there
- Run `restore-from-file`

or
- Run `restore-from-remote` with appropriate arguments to download and restore

Your site should be restored. The subdomain of the URL will be the
same as the name of the project directory. So will the database, user
and password. (As we are doing this locally, security is not an
issue.)

## CiviCRM

CiviCRM can be used as a plugin in Joomla, but it has its own
configuration which contains references to the file path and URL where
the site is installed so this needs to be tweaked.

In this repo there is also a scripts that will do this tweaking after a
restore. This will be taken care of automatically if you use one of the
`restore-civicrm-from-remote` or `restore-civicrm-from-file` scripts.

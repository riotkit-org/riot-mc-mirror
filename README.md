riot-mc-mirror
==============
Scheduled Min.io mirroring using **Min.io Client mirror option with crontab**. Simple.
Container does first mirroring when starting, to validate if the configuration parameters are valid.
**ARM image available**

# QA
Q: Why additional container, not directly `minio/mc`?
A: For following reasons:
- Easily use crontab
- Configure mc connection easily with environment variables
- No official ARM container

Example usage
-------------

```yaml
version: "2"
services:
    mirroring_comrade:
        image: quay.io/riotkit/riot-mc-mirror:2019-09-11T20-17-47Z-x86_64
        environment:
            # when (in crontab syntax)
            - CRON_SCHEDULE=*/10 * * * *
            - MIRROR_OPTS=--debug  # optional Min.io Client options

            - SOURCE_URL=http://primary.backups.example.org
            - SOURCE_BUCKET=backup
            - SOURCE_ACCESS_TOKEN=123
            - SOURCE_SECRET=456

            - TARGET_URL=http://replica-1.backups.example.org
            - TARGET_BUCKET=backup
            - TARGET_ACCESS_TOKEN=123
            - TARGET_SECRET=456
```

Configuration reference
-----------------------

List of all environment variables that could be used.

```yaml

- SOURCE_URL # (default: http://primary.backups.example.org)

# Access token to access resources on source S3
- SOURCE_ACCESS_TOKEN # (default: some-token)

# Secret to access resources on source S3
- SOURCE_SECRET # (default: some-secret)

# Bucket name from which the data will be copied from
- SOURCE_BUCKET # (default: test-src-bucket)

# URL of the target S3 where the files will be synchronized to
- TARGET_URL # (default: http://some-minio-instance)

# Access to the copy target
- TARGET_ACCESS_TOKEN # (default: test)

# Access to the copy target
- TARGET_SECRET # (default: test)

# Bucket name on target S3
- TARGET_BUCKET # (default: test-bucket)

# Options passed to "mc mirror" command
- MIRROR_OPTS # (default: --json)

# Crontab schedule
- CRON_SCHEDULE # (default: "*/5 * * * *")


```

Developing the container
------------------------

- The container is built on quay.io and hub.docker com
- When you start working on it locally, at first run `make dev@develop` to install git hooks
- README.md is automatically generated from README.md.j2, do not edit the generated version!
- Use `make` for building, pushing, etc.

Releasing
---------

On Travis CI the build is triggered each month, then all recent versions of Min.io Client are built. Already existing docker tags are not overwritten.
The build is also triggered on-commit.

To release a bugfix version and REBUILD EXISTING TAGS just add "@force-rebuild" in commit message, recent 3 tags will be rebuilt (not all in registry).

Copyleft
--------

Created by **RiotKit Collective**, a libertarian, grassroot, non-profit organization providing technical support for the non-profit Anarchist movement.

Check out those initiatives:
- International Workers Association (https://iwa-ait.org)
- Federacja Anarchistyczna (http://federacja-anarchistyczna.pl)
- Związek Syndykalistów Polski (https://zsp.net.pl) (Polish section of IWA-AIT)
- Komitet Obrony Praw Lokatorów (https://lokatorzy.info.pl)
- Solidarity Federation (https://solfed.org.uk)
- Priama Akcia (https://priamaakcia.sk)

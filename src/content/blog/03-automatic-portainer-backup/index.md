---
title: "Automatically Backup Portainer"
description: "Securely backup the Portainer configuration."
date: "2025-01-01"
draft: false
---

It is always a good practise to backup stuff. This is especially the case when running a
homelab server. Sometimes things just go wrong and it is great to be able to restore
to a previous state. For most of my Docker applications this is quite staightforward.
Every night a script is executed which creates a backup of the files and database. The backups are handeled by restic which takes care of deduplication and managing the history of snapshots.

#### Why Backup Portainer?

One container I have not backed up so far is Portainer. This is because I thought that it does not really hold any valuable data and is just used to manage my already backed up containers. But this is not entirly correct. It does hold the docker-compose setup of all containers and a lot of evironment variables which I don't want to lose. So Portainer should also be backed up.

#### Backup Portainer

The first thing you can do is to go to the Portainer settings in the web UI and scroll to the backup section. There you can create and directly download a full backup. The only caveat is that, unless you are using the business plan, you cannot schedule a backup.

![Backup Portainer via web UI](/blog-3/backup-portainer-web-ui.png)

But there is a workaround, the [Portainer API](https://app.swaggerhub.com/apis/portainer/portainer-ce/2.21.5). For this a JWT is required.
So in total we are going to need two API requests.


Firstly, to get the JWT and write it to a file called `.jwt` we run:

```bash
wget https://your_domain/api/auth \
--post-data '{"username": "your_username", "password": "your_password"}' \
-O .jwt
```

And then to create and download the backup:

```bash
wget https://your_domain/api/backup \
--header "Authorization: Bearer $(cat .jwt | jq -r .jwt)" \
--post-data '{"password": ""}' \
-O portainer_backup.tar.gz
```

The command `jq` is used to extract the JSON data. It can be installed with:

```bash
sudo apt update
sudo apt install jq
```

As the JWT expires after a few hours both commands need to be executed to create a backup.
These commands can easily be put into a cronjob and now you can automatically backup Portainer.
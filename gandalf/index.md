# Gandalf

![Gandalf image](https://gitlab.yoomee.com/yoomee/docs/raw/master/assets/images/gandalf.png)

Gandalf is a DigitalOcean droplet. Log in at [digitalocean.com](https://www.digitalocean.com/) with `developers@yoomee.com` and password key `DIGITALOCEAN`.

## SSH access

`ssh root@gandalf.yoomee.com` with password key `GANDALF`.

Optionally add your SSH key to the server so you don't have to keep typing in the password:

```
cat ~/.ssh/id_rsa.pub | ssh root@gandalf.yoomee.com "cat >> /root/.ssh/authorized_keys"
```

## GitLab

The DigitalOcean droplet came preinstalled with GitLab Community Edition (CE), installed into `/home/git`. We have since upgraded GitLab with new releases. A new version of GitLab CE is released on the 22nd of every month. The changelog and upgrade instructions can be found at [about.gitlab.com/blog](https://about.gitlab.com/blog/).

A copy of the GitLab nginx config is in [nginx.gitlab.conf](nginx.gitlab.conf).

## Gem server

The gem server at [gems.yoomee.com](https://gems.yoomee.com) is hosted on Gandalf in `/home/gems`. The server config files are in the [geminabox-server](https://gitlab.yoomee.com/yoomee/geminabox-server/tree/master) repo and we have a Yoomee fork of the Geminabox gem [here](https://gitlab.yoomee.com/yoomee/geminabox/tree/master).

A copy of the Geminabox nginx config is in [nginx.geminabox.conf](nginx.geminabox.conf).

## Slackbot schedule

The slackbot scheduled reminders of stretch o'clock, beer o'clock etc are triggered by cron jobs on Gandalf. See [yoomee/schedule](https://gitlab.yoomee.com/yoomee/schedule/tree/master) for more information.

## Backup of old projects

There are some old projects backed up on Gandalf that aren't yet on Gitlab. See [recovering backups](recovering_backups.md).


## Other docs
* [Documentation of the initial setup process of Gandalf](initial_setup_and_migration_from_pippin.md)
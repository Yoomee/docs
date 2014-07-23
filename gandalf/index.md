# Gandalf

![Gandalf image](https://gitlab.yoomee.com/yoomee/docs/raw/master/assets/images/gandalf.png)

Gandalf is a DigitalOcean droplet. Log in at [digitalocean.com](https://www.digitalocean.com/) with `developers@yoomee.com` and password key `DIGITALOCEAN`.

## SSH access

`ssh root@gandalf.yoomee.com` with password key `GANDALF`.

## GitLab

The DigitalOcean droplet came preinstalled with GitLab Community Edition (CE), installed into `/home/git`. We have since upgraded GitLab with new releases. A new version of GitLab CE is released on the 22nd of every month. The changelog and upgrade instructions can be found at [about.gitlab.com/blog](https://about.gitlab.com/blog/).

A copy of the GitLab nginx config is in [nginx.gitlab.conf](nginx.gitlab.conf).

## Gem server

The gem server at [gems.yoomee.com](https://gems.yoomee.com) is hosted on Gandalf. We have a Yoomee fork of the Geminabox gem [here](https://gitlab.yoomee.com/yoomee/geminabox).x

A copy of the Geminabox nginx config is in [nginx.geminabox.conf](nginx.geminabox.conf).


## Further docs
* [Recovering old projects that aren't on Gitlab](recovering_backups.md)
* [Documentation of the initial setup process of Gandalf](initial_setup_and_migration_from_pippin.md)
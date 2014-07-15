#Gandalf setup

### Upgrade and configure GitLab

* Created GitLab droplet
* Added my SSH key
* SSHed in and updated GitLab to latest version
* Modify `config/gitlab.yml` to set host, email addresses and default settings
* Copy self-signed certificate and key into place. Modify `/etc/nginx/sites-available/gitlab` to support SSL and redirect non-SSL

### Import GitLab data

* Run `cd /home/git/gitlab && sudo -u git -H RAILS_ENV=production bundle exec rake gitlab:backup:create` on Pippin
* scp backup file to Gandalf in Rails.root/tmp/backups
* From `/home/git/gitlab` try `sudo -u git -H bundle exec rake gitlab:backup:restore RAILS_ENV=production` and checkout the commit hash it suggests
* `mv config/initializers/rack_attack.rb config/initializers/rack_attack.rb.example` out the way for now to prevent error
* Run `sudo -u git -H bundle exec rake gitlab:backup:restore RAILS_ENV=production` again
* `mv config/initializers/rack_attack.rb.example config/initializers/rack_attack.rb`
* `sudo chown -R git:git *` if needed
* `sudo -u git -H git checkout 6-7-stable`
* `sudo -u git -H bundle exec rake db:migrate RAILS_ENV=production`, might need to comment out the innards of `20131112114325_create_broadcast_messages.rb`, `20131130165425_add_color_and_font_to_broadcast_messages.rb`, `20140122112253_create_merge_request_diffs.rb`, `20140209025651_create_emails.rb` and`20140304005354_add_index_merge_request_diffs_on_merge_request_id.rb`
* `sudo service gitlab restart`


### Gem server
* Added gems user to Gandalf with `sudo adduser --disabled-login gems`
* Got data and geminabox directories from pippin /home/geminabox
* Removed bloat, cached stuff, change config for gandalf
* Transfer to gandalf /home/gems
* `sudo chown -R gems:gems /home/gems`
* `cd /home/gems/geminabox` and `bundle`
* Transfer over SSL cert, init.d script and nginx conf.
* `ln -s /etc/nginx/sites-available/geminabox /etc/nginx/sites-enabled/geminabox`
* Modified /etc/nginx/sites-available/gitlab to only listen for gitlab.yoomee.com
* Start geminabox on boot `sudo update-rc.d geminabox defaults 21`
* Start geminabox `sudo service geminabox start`
* Visit `https://gems.yoomee.com/reindex` to reindex

### Other bits
* Moved schedule (slackbot)
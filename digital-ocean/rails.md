# Digital Ocean

This README explains how to setup a cloud server on Digital Ocean that uses Ruby on Rails, Postgress and Capistrano.

Please follow all the steps below in order.

## Create Droplet

Login to https://www.digitalocean.com with the developers@yoomee.com account.
Get the password from the usual palce.

1. Click on the big "Create" button to create a new server. They are called 'Droplets'
- Name the droplet something sensible (eg. 'billable-machine')
- Select size (The smallest will be fine for starters.)
- Select Region as London.
- Leave ‘Available settings’ blank
- Select Image -> Linux Distribution -> Ubuntu 14.04 x32
- Don’t select applications
- Don’t add SSH keys
- Click ‘Create droplet’
- Wait for root password to be emailed to developers@yoomee.com

## Change root password and add deploy user

SSH into the server ip address using the password contained in the email address
and change the root password to something secure.
Replace IPADDRESS with the one for your server:
```
$ ssh root@IPADDRESS
$ passwd
```

Remember to share the new password with the rest of the team in the usual place.

### Now add new user to server

The first thing we will do on our new server is create the user account we'll be using to run our applications and work from there.

Create the user. Make sure you enter a secure password and share that in the usual place.

```
$ sudo adduser deploy
```

Add the new user to the sudo group:

```
$ sudo adduser deploy sudo
```

Check that is works, by switching to the new user:

```
$ su deploy
```

## Add your SSH keys

Next we are going to add the SSH key from your local machine so that you don't need to login each time with a password.

We're going to use ssh-copy-id on your local compuer to set this up:

```
$ brew install ssh-copy-id
```

Note: Do this on your local computer!

Once you've got ssh-copy-id installed, run the following and replace IPADDRESS with the one for your server:

Make sure you run ssh-copy-id on your computer, and NOT the server.

```
$ ssh-copy-id deploy@IPADDRESS
```

Enter the secure password you entered above.

Now when you run ssh deploy@IPADDRESS you will be logged in automatically. Go ahead and SSH again and verify that it doesn't ask for your password before moving onto the next step.

```
ssh deploy@IPADDRESS
```

Boom no password!

## Add 1024MB swap file

We need to add a swap file, especially to help the performance when we have less than 1Gb RAM.

Before we proceed to set up a swap file, we need to check if any swap files have been enabled on the VPS by looking at the summary of swap usage.

```
$ sudo swapon -s
```

An empty list will confirm that you have no swap files enabled:

```
Filename                    Type          Size     Used     Priority
```

We can check how much space we have on the server with the df command. The swap file will take 1024MB.

```
$ df -h
```

Create the swap file:

```
$ sudo dd if=/dev/zero of=/swap bs=1M count=1024
$ sudo mkswap /swap
$ sudo swapon /swap
```

Check the swap file was created:

```
$ swapon -s
```

Ensure that the swap is permanent by adding it to the fstab file.

Open up the file:

```
$ sudo vi /etc/fstab
```

Paste in the following line:

```
 /swap       none    swap    sw      0       0
```

Swappiness in the file should be set to 10. Skipping this step may cause both poor performance, whereas setting it to 10 will cause swap to act as an emergency buffer, preventing out-of-memory crashes.

You can do this with the following commands:

```
$ echo 10 | sudo tee /proc/sys/vm/swappiness
$ echo vm.swappiness = 10 | sudo tee -a /etc/sysctl.conf
```

To prevent the file from being world-readable, you should set up the correct permissions on the swap file:

```
$ sudo chown root:root /swap
$ sudo chmod 0600 /swap
```

## Install  RVM


Before we do anything else, we should run a quick update to make sure that all of the packages we download to our virtual server are up to date

```
$ sudo apt-get update
```

To install RVM, open terminal and type in this command:


```
$ curl -L get.rvm.io | bash -s stable
$ source ~/.rvm/scripts/rvm
```

Install other dependencies:

```
$ rvmrequirements
$ rvmsudo /usr/bin/apt-get install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison subversion
```

## Install Ruby

```
$ rvm install 1.9.3
$ rvm use 1.9.3 default
$ rvm install 2.1.1
```

## Install RubyGems

Check we have all the dependencies:

```
$ rvm ruby gems current
```

## Install Rails

```
$ gem install rails
```

This process may take a while, be patient with it. Once it finishes you will have Ruby on Rails installed on your virtual server.
Once that’s done you are all set with Ruby on Rails, and it is time to connect it to nginx.

We also need nodejs:

```
$ sudo apt-get install nodes
$ sudo apt-get install ruby-railties-4.0
```

## Install Passenger

Passenger is an effective and easy way to deploy Rails on nginx.

Install as follows:

```
$ gem install passenger
```

Also install Curl development headers with SSL support:

```
$ apt-get install libcurl4-openssl-dev
```

## Install nginx

We are looking to install Rails on an nginx server, we only need to enter one more line into terminal:

```
$ rvmsudo passenger-install-nginx-module
```

Select Rails, Python, Node.js

Select 1. “Yes: download, compile and install Nginx for me.”
Accept defaults.

Passenger first checks that all of the dependancies it needs to work are installed. If you are missing any, Passenger will let you know how to install them, either with the apt-get installer on Ubuntu.

After you download any missing dependancies, restart the installation. Type: rvmsudo passenger-install-nginx-module once more into the command line.

Passenger offers users the choice between an automated setup or a customized one. Press 1 and enter to choose the recommended, easy, installation.

This will install nginx to /opt/nginx

Passenger will take about five to ten minutes to install, configure, and optimize nginx with Ruby on Rails.

After it finishes, it will let you know about changes made to the nginx configuration file and how to deploy a Ruby on Rails application on your virtual server.

### Create nginx startup scripts

Download nginx startup script

```
$ wget -O init-deb.sh http://library.linode.com/assets/660-init-deb.sh
```

Move the script to the init.d directory & make executable

```
$ sudo mv init-deb.sh /etc/init.d/nginx
$ sudo chmod +x /etc/init.d/nginx
```

Add nginx to the system startup:

```
$ sudo /usr/sbin/update-rc.d -f nginx defaults
```

Now we can control nginx using:

```
$ sudo service nginx stop
$ sudo service nginx start
$ sudo service nginx restart
$ sudo service nginx reload
```

## Connect Nginx to Your Rails Project

Once you have rails installed, open up the nginx config file

```
$ sudo vi /opt/nginx/conf/nginx.conf
```

Set the root to the public directory of your new rails project.

Your config should then look something like this, so that it can work with capistrano paths:

```
server {
   listen 80;
   server_name example.com;
   passenger_enabled on;
   root /home/deploy/railsapp/current/public;
}
```
We are going to install our Rails app into a directory called 'railsapp'.

The last step is to turn start nginx, as it does not do so automatically.

```
$ sudo service nginx start  
```

## Install Postgres

First make sure we have the latest packages:

```
$ sudo apt-get update
```

Install Postgres:

```
$ sudo apt-get install postgresql postgresql-contrib libpq-dev
```

The installation procedure created a user account called postgres that is associated with the default Postgres role. In order to use Postgres, we'll need to log into that account. You can do that by typing:

```
$ sudo -i -u postgres
$ psql
```

### Create deploy user

We are doing to use the 'deploy' user with a blank password for our rails app, so we need to create this role:

```
create role deploy with superuser createrole createdb replication login password '';
\q
```

We also need change permissions as follows to avoid problems with Rails:

```
$ sudo vi /etc/postgresql/9.3/main/pg_hba.conf
```

To allow rails to connect, simply change the bottom of pg_hba.conf to look like this.

```
# TYPE  DATABASE    USER        CIDR-ADDRESS          METHOD

# "local" is for Unix domain socket connections only
local   all         all                               trust
# IPv4 local connections:
host    all         all         127.0.0.1/32          trust
# IPv6 local connections:
host    all         all         ::1/128               trust
```

Then restart postgres:

```
$ sudo service postgresql restart
```

Test with rails app

```
$ cd /home/deploy/
$ rails new railsapp --database=postgresql
```

Change the password and username to the following in config/database.yml:

```
production:
  <<: *default
  database: railsapp_production
  username: deploy
  password:
```

```
$ RAILS_ENV=production rake db:create
```

That should create a database in Postgres successfully.

## Add SSH keys to GitLab

In order for the new server to be able to grab code from Gitlab when you deploy,
then you will need to add the server's SSH key to Gitlab as follows:

##### Step 1. First, we need to check for existing SSH keys on the server.

```
$ ls -al ~/.ssh
```

Check the directory listing to see if you have files named either id_rsa.pub or id_dsa.pub. If you don't have either of those files, go to step 2. Otherwise, skip to step 3.

##### Step 2. Generate new keys:

```
$ ssh-keygen -t rsa -C "SERVERNAME@yoomee.com"
```

Replace SERVERNAME to the name of the server (eg. 'billable-machine')


Press enter to accept all the default values.

##### Step 3. Copy the key

Let's see the contents of the SSH key:

```
$ cat ~/.ssh/id_rsa.pub
```

Now cut and paste the contents and add it to Github project under ‘settings -> deploy keys’ for the project.

## Setup Capistrano

There is nothing special to setup on the server for automated Capistrano deploys.

Just make sure that your Rails app has the following files that are pushed to Git:

### Capfile

```
require 'capistrano/setup'
require 'capistrano/deploy'
require 'capistrano/rails'
require 'capistrano/rvm'
set :rvm_type, :user
set :rvm_ruby_version, '2.1.1'
Dir.glob('lib/capistrano/tasks/*.rake').each { |r| import r }
```

### config/deploy.rb

```
lock '3.2.1'
set :application, 'billable-machine'
set :repo_url, 'git@gitlab.yoomee.com:yoomee/billable-machine.git'
set :deploy_to, '/home/deploy/railsapp'
set :linked_dirs, %w{bin log tmp/pids tmp/cache tmp/sockets vendor/bundle public/system}

namespace :deploy do
  desc 'Restart application'
  task :restart do
    on roles(:app), in: :sequence, wait: 5 do
      execute :touch, release_path.join('tmp/restart.txt')
    end
  end
  after :publishing, 'deploy:restart'
  after :finishing, 'deploy:cleanup'
end
```

### config/deploy/production.rb

```
set :stage, :production
# Replace with your server's IP address!
server 'IPADDRESS', user: 'deploy', roles: %w{web app}
```

Note: Replace IPADDRESS aboce with the server's IP address,



### Gemfile

Add the following to your Gemfile:

```
gem 'capistrano', '~> 3.2.0'
gem 'capistrano-bundler', '~> 1.1.2'
gem 'capistrano-rails', '~> 1.1.1'
gem 'capistrano-rvm', github: "capistrano/rvm"
```

## Automated deploy

You can now automatically deploy your Rails app to the new Digital Ocean server by using the following commands on your local machine:

```
cap production deploy
```

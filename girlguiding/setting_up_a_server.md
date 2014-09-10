#Set up a gg server for deploy

Assuming the server is at 10.0.14.**

ssh into the server as root - the password is in the usual place

`ssh root@10.0.14.**`

Add a deploy user - with the password from the usual place

`adduser deploy`

Add user to the sudo group

`usermod -aG sudo deploy`

Install git

`sudo apt-get install git`

Set up the directories for the rails app - follow the directions in the Authorisation section of the capistrano docs http://capistranorb.com/documentation/getting-started/authentication-and-authorisation/

Exit

`exit`

Add your own keys as authorized on the server - so on your own machine

`cat ~/.ssh/id_rsa.pub | ssh deploy@10.0.14.** "mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys"`

Then you can ssh without a password

`ssh deploy@10.0.14.**`

Generate a keypair for the new server - no passphrase

`ssh-keygen -t rsa`

Then copy the key into gitlab as a deploy key - this will give you the text to copy - be careful

`cat ~/.ssh/id_rsa.pub`

Exit

`exit`

While the yoomee gems are still git references, rather than gems, you'll need to add it to all the gems, as well as the main repository.

Copy the .env file from an existing server

`scp deploy@10.0.14.10:/var/www/girlguiding/shared/.env deploy@10.0.14.**:/var/www/girlguiding/shared/.env`

###TODO set up ruby postgres and anything else that needs setting up

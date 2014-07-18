# Heroku

## Domains and subdomains

### Subdomain
1. Add the subdomain to the list in the settings tab for your app on heroku.
2. Add a CNAME for the subdomain on http://heartinternet.uk pointing to the app on heroku - it needs a trailing . eg ```styleguiding.herokuapp.com.```

### Domain

As above, point www to heroku, then

1. Add an automatic redirect in the webforwarding control area of http://heartinternet.uk to eg ```www.styleguiding.org.uk```. 
  - It looks as though you are forwarding www to www, but it works for the naked domain too.

# Deploying an application to Heroku

#### Add Heroku gems

```
Add 'gem rails_12factor' to the Gemfile and bundle
```

#### Create a new Heroku application

```
heroku create <appname>
```


#### Deploy

If the application is using Yoomee gems in gitlab, the Gems will need to be packaged before pushing to Heroku

```
bundle package --all
```

Add the resulting vendor/cache directory to Git


```
git add vendor/cache
git commit -m "Updated vendor cache"
```

If you are using Heroku to host a staging environment you may wish to use push a branch other than master

```
git push heroku <branch-name>:master
```

#### Gotcha

If you get this error message:

```
fatal: 'heroku' does not appear to be a git repository
fatal: Could not read from remote repository.
```

Then add a remote as follows:

```
git remote add heroku git@heroku.com:girlguiding-staging.git
```
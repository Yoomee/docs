# Recovering backups

Backups of the old Yoomee office git server - dev1 - were made and are stored on gandalf. To recover files from it:

* ssh to the server and find what you want

```
ssh root@gandalf.yoomee.com
```

* log out of the server and copy the file/files to your local machine

```
 scp root@gandalf.yoomee.com:/home/yoomee/backup/whatever_you_want .
```

## If you're recovering a git repository
If it's a git repostitory, then you can clone directly from the saved file.
For example, to clone yoomee-moderation

```
git clone ~/Rails/Gems/yoomee-moderation.git
```

Then remove the current local git remote, and add a new one on gitlab (or somewhere else) and push to the new remote

```
git remote remove origin
git remote add origin git@gitlab.yoomee.com:whatever_you_want.git
git push -u origin master 
```





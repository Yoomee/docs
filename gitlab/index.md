# Yoomee Gitlab

### Introduction

We use our own Git respository on a server running GitLab Community Edition at https://gitlab.yoomee.com on a server called [Gandalf](https://gitlab.yoomee.com/yoomee/docs/blob/master/gandalf/index.md).

### Setup

Your colleagues should have created you an administrator account. If you forgot the password then grab a new password [here](https://gitlab.yoomee.com/users/confirmation/new). When you are signed in, you will need to add your SSH keys in order to use Gitlab.


### Add your SSH keys

#### Step 1: Check for SSH keys

First, we need to check for existing SSH keys on your computer. Open up your Terminal and type:

```
$ ls -al ~/.ssh
```

Check the directory listing to see if you have files named either **id_rsa.pub** or **id_dsa.pub**. If you don't have either of those files, go to step 2. Otherwise, skip to step 3.

#### Step 2: Generate a new SSH key

```
$ ssh-keygen -t rsa -C "your_email@example.com"
```

Note: Just hit enter on all the questions.


#### Step 3: Add your SSH key to GitLab

Run the following code to copy the key to your clipboard.

```
$ pbcopy < ~/.ssh/id_rsa.pub
```

Now that you have the key copied, it's time to add it into GitLab as follows:

1. Sign in to Gitlab with you email addres and password.
2. Click on "Profile settings" in the top right.
3. Click on "SSH Keys" in the top menu bar.
4. Click on button "Add SSH Key".
5. In the "Title" field, add a descriptive label for the new key. For example, if you're using a personal Mac, you might call this key "Personal MacBook Air".
6. Paste your key into the "Key" field.
7. Click on the "Add key" button

## Gitlab groups

You will need to add yourself to the various groups of projects you are working on.

## General tips

There is a simple guide here http://git.huit.harvard.edu/guide/ otherwise Google is your friend.

---
layout: post
title: "Password management for teams"
date: 2013-02-21 00:27
comments: true
categories: 
---
We've been looking for a proper password management tool for our team quite
some time.  Even if we focus on Single-Sing-On wherever possible, you
have a good collection of service users or privileged users.

Our requirements (beside security) were

+ usability
+ no shared passwords
+ access control
+ easy to integrate in our work flows

Based on our style of work we were searching exclusively for command
line tools.

You are right - since I write this blog post we've already found our password management tool: _pass_

``` bash
$ brew install pass  # Depends on: xz, pwgen, tree, gnu-getopt, gnupg2
```

Pass is based on GnuPG, so you encrypt, decrypt and trust based on your
GPG keyrings.  The documentation of pass shows the usage for a
single user workflow.  The most simple adaption for teams is to use GPG groups. 

Per default GPG groups are defined in the local configuration file.  So if you
don't changed your team members often this perhaps convenient enough for
you.  If not I would search for a way to use includes in the
configuration
file, where you put the fragment itself in the repository.

``` bash
$ gpg2 --gen-key     # in case you need a new or separate keyring
```

If you are located behind a firewall that blocks port 11371/tcp you should
change the GnuPG keyserver communication to port 80/tcp:

``` bash
$ grep ^keyserv ~/.gnupg/gpg.conf
keyserver  hkp://p80.pool.sks-keyservers.net:80

$ gpg2 --send-keys <keyid>
$ gpg2 --search-keys <team-members>
```

After the typical key signing and the group setup we are ready to create the
password repository.

``` bash
$ pass init <gpg_group_name>
$ pass git init
$ pass git remote add origin myserver.net:pass-store
```

Now you can store and retrieve keys with pass.  Have a look at the man
pages or the [web site](http://zx2c4.com/projects/password-store/).

Let me know if you like this approach or you decided to go for another one - feedback welcome.

### Links

+ [pass - the standard unix password manager](http://zx2c4.com/projects/password-store/)
+ [GnuPG](http://www.gnupg.org)
+ [Git](http://git-scm.com)


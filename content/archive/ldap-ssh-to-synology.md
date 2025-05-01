---
title: "Enable LDAP User SSH to Synology NAS"
date: 2021-07-12T08:00:00+01:00
draft: false
categories: ["devops"]
tags: ["ldap","security","ssh","synology"]
series: ["archive"]
---

Users created with [Synology's LDAP Server][1] have their login shell set to /sbin/nologin. Add the following to the end of /usr/syno/etc/nslcd.conf if you want to enable SSH logins to your NAS for your LDAP users:

```
map passwd loginShell "/bin/sh"
```

Then run the following command:

``` bash
$ synoservice --restart nslcd
```

Your LDAP users should now be able to SSH into the Synology NAS, in this case using a password-less SSH key:

``` bash
$ ssh ldaptest@synology
ldaptest@synology:~$ exit
logout
Connection to synology closed.
```

[1]: https://kb.synology.com/en-af/DSM/help/DirectoryServer/ldap_desc?version=6

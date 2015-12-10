# RADStack
Research And Development Stack

## Introduction
This project is intended to set up a full stack of web based tools to aid in Research and Development projects/teams, including the ability to make/restore backups, and cleanly remove itself from the host server.  It is intended to scale from one researcher/developer to large teams developing multiple projects.

RADStack includes the following capabilities, in layers that are abstractly named such that other solutions can be dropped in if better solutions to each need is found.  These needs are listed here, with the implemented solution.
* User Authentication - OpenLDAP/PHPLDAPAdmin
* DataBase - MySQL/PHPMYADMIN
* Software Configuration Managment (SCM) - Gerrit
* Content Managment (CM) - Wordpress
* Continuous Integration (CI) - Jenkins
* Project Managment (PM) - Bugzilla

## Installation
1. Clone this repository.  Ensure to get the submodules.

   ```bash
   >> git clone --recursive https://github.com/bellockk/RADStack.git
   ```
2. Edit the `Define Parameters` section of the bootstrap script (located at the top of the script) to include your server settings.  Note that for the remainder of this installation guide, I will refer to the default entries in the script.  If you have modified the default entries, use your edited entries in the steps below.
3. Log into the host machine and edit your `/etc/hosts` file to include the following entries.  Note that if you have a DNS server, you can just add these sites to your DNS table instead.

   ```bash
   127.0.0.1		example.com
   127.0.0.1		scm.example.com
   127.0.0.1		ci.example.com
   127.0.0.1		cm.example.com
   127.0.0.1		pm.example.com
   127.0.0.1		ldapadmin.example.com
   127.0.0.1		dbadmin.example.com
   ```
4. Run the `bin/bootstrap' script.  Note that docker must be installed on the server, and the user who executes this script must be a member of the docker group.

   ```bash
   >> cd RADStack
   >> bin/bootstrap
   ```
5. Configure LDAP and Add the first user.
   1. Open a web browser and go to http://ldapadmin.example.com
   2. On the left side of the web page under `myldap` click the `login` link.
   3. In the `Password:` entry field, enter `secret`, and click the `Authenticate` button.
   4. In the left frame under `myldap`, click the `+` next to the line marked `dc=ldap,dc=example,dc=com`.
   5. Click the `Create new entry here` link.
   6. In the right frame labeled `Templates:` click `Generic: Organisational Unit`.
   7. In the entry field labeled `Organisational Unit`, enter `groups` and push the `Create Object` button.
   8. Click the `Commit` button.
   9. In the right frame labeled `ou=groups` click the `Create a child entry` link.
   10. In the right frame labeled `Templates:` click `Generic: Posix Group`.
   11. In the entry field labeled `Group` enter `users` and click `Create Object`.
   12. Click the `Commit` button.
   13. In the left frame labeled `myldap` click the `Create new entry here` link.
   14. In the right frame labeled `Templates:` click `Generic: Organisational Unit`.
   15. In the entry field labeled `Organisational Unit`, enter `people` and push the `Create Object` button.
   16. Click the `Commit` button.
   17. In the right frame labeled `ou=people` click the `Create a child entry` link.
   18. In the right frame labeled `Templates:` click `Generic: User Account`.
   19. In the entry field labeled `First name` enter your first name.
   20. In the entry field labeled `Last name` enter your last name.
   21. In the entry field labeled `Password` enter your password.
   22. In the option menu labeled `GID Number` select `users`.
   23. In the option menu labeled `Login shell` select any shell, and click `Create Object`.
   24. Click the `Commit` button.
   25. In the right frame, click the `Add new attribute` link.
   26. From the option menu that appears select `Email`, enter the users email, and click `Update Object` at the bottom of the screen.
   27. Click the `Update Object` button.
   28. In the left frame under `myldap`, click the `+` sign next to `ou=groups`, and click the link labeled `cn=users`.
   29. In the right frame labeled `cn=users` click the link labeled `Add new attribute`.
   30. From the option menu that appears select `memberUid`, enter `1000` in the entry field, and click `Update Object` at the bottom of the screen.
   31. Click the `Update Object` button.
6. Configure Software Configuration Management.
   1. Go to http://scm.example.com in a web browser.
   2. Click on the `Sign In` link in the top right hand corner of the page.
   3. Enter the userid and password of the account you created above.  This user is now the gerrit site administrator.
7. Bind Continuous Integration to LDAP
   1. Go to http://ci.example.com in a web browser.
   2. Click on `Manage Jenkins` on the left side of the page.
   3. Click on the `Setup Security` button near the top right of the page.
   4. Check the box labeled `Enable security`.
   5. In the fields that open up, under `Security Realm` check `LDAP`.
   6. In the next field that opens up, labeled `Server` enter `ldap://myldap` and click the `Advanced` button.
   7. In the `root DN` entry field, enter `dc=ldap,dc=example,dc=com`
   8. In the `User search base` entry field, enter `ou=people`
   9. In the `User search filter` entry field, enter `uid={0}`
   10. In the `Group search base` entry field, enter `ou=groups`
   11. In the `Group search filter` entry field, enter `cn={0}`
   12. In the `Manager DN` entry field, enter `cn=admin,dc=ldap,dc=example,dc=com`
   13. In the `Manager Password` entry field enter `secret`
   14. In the `Display Name LDAP attribute` entry field, enter `cn`
   14. In the `Email Address LDAP attribute` entry field, enter `Email`
   15. In the `Authorization` group, check `Logged-in users can do anything`
   16. Click the `Save` button at the bottom of the screen.
8. Bind Content Managment to LDAP (Under Development)
   1. Go to http://cm.example.com in a web browser.
   2. Click the `Continue` button.
   3. Enter an approriate value in the `Site Title` entry field.
   4. Enter a temporary username/password/email in the appropriate fields and click the `Install WordPress` button.
   5. Click the `Log In` button.
   6. Use your temporary username/password to log in.
   7. Click on the `Plugins` link on the frame on the left side of the page.
   8. Next to the `Plugins` title, click the `Add New` button.
   9. In the `Search Plugins` entry field, enter `LDAP` and press enter.


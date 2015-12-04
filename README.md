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

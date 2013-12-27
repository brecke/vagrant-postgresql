# Postgresql database server whenever you need one

## Dependencies

You'll need to have the following tools installed for this to work

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* [Vagrant](http://vagrantup.com/)

## Instructions

This repo contains everything necessary to stand up a local instance of an
ubuntu 12.04 server running postgresql and phppgadmin.

To get the server going execute the following in the directory:

    script/database up

To shutdown the database server do the following:

    script/database down

To destroy the database server do the following (this will remove all data):

    script/database destroy

Then you should be able to navigate to http://localhost:8080/phppgadmin and log
into the database server using the username *postgres* and password *password*

From a terminal you can also start a shell (given that you have the postgresql
client on your host operating system).

    psql -h localhost -U postgres --password
    enter password: password
    
## Fixing locale settings

    $ sudo nano /etc/bash.bashrc

    # Add these lines to the bottom of the file:
    export LANGUAGE=en_US.UTF-8
    export LANG=en_US.UTF-8
    export LC_ALL=en_US.UTF-8

    $ sudo locale-gen en_US.UTF-8
    $ sudo dpkg-reconfigure locales
    
    # reload vagrant
    
If these instructions don't allow you to create a UTF8 database, then follow these instructions:

Create a file

		nano /etc/profile.d/lang.sh

		# Add the following
		export LANGUAGE="en_US.UTF-8"
		export LANG="en_US.UTF-8"
		export LC_ALL="en_US.UTF-8"

Save it. Restart shell or run all export commands manually in current shell instance
Reconfigure so the encoding can be UTF8:

		sudo su postgres
		psql
		update pg_database set datistemplate=false where datname='template1';
		drop database Template1;
		create database template1 with owner=postgres encoding='UTF-8'
		lc_collate='en_US.utf8' lc_ctype='en_US.utf8' template template0;
		update pg_database set datistemplate=true where datname='template1';

Then use template1 for db creation. ([source](http://stackoverflow.com/questions/13115692/encoding-utf8-does-not-match-locale-en-us-the-chosen-lc-ctype-setting-requires))

## Troubleshooting

This vagrant box takes ports 5432 and 8080 so if you have processes listening
on those ports then things might not work.


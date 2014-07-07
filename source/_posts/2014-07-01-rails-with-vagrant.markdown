---
layout: post
title: "Rails with Vagrant"
date: 2014-07-01 12:09:07 -0400
comments: true
categories: Rails Vagrant
---

<p>Vagrant makes setting up a development environment simple. Even more, the environments
can be shared to reduce any compatibility issues with a user's personal setup.
More often than not, when I clone a project, I have to spend a good deal of time
getting all the components of a project to work with my setup. I'm a fan of cloning
a project, running <code>vagrant up</code> and getting started immediately. Vagrant
is the golden sandbox that has me a bit too thrilled.</p>

<p>The folks at JumpstartLab put out an excellent
<a href="http://tutorials.jumpstartlab.com/topics/vagrant_setup.html">tutorial</a> on
getting Vagrant setup with all the basic necessities. One issue I continued to run
into though was a language error with Postgres when trying to run
<code>rake db:create </code>. The error read:

```
PG::InvalidParameterValue: ERROR:  encoding UTF8 does not match locale en_US
DETAIL:  The chosen LC_CTYPE setting requires encoding LATIN1.
: CREATE DATABASE "platform_validator_development" ENCODING = 'utf8'

```
</p>
<p>
This tends to happen when the locale isn't set before creating a database instance
inside vagrant. Digging deeper into the issue, the
<a href="http://www.postgresql.org/docs/8.2/static/locale.html">Postgres doc</a> states:
{% blockquote %}
"Note that the locale behavior of the server is determined by the environment variables seen by the server, not by the
environment of any client. Therefore, be careful to configure the correct locale
settings before starting the server."
{% endblockquote %}
</p>
<p>
The peculiar thing about Ubuntu is that when you execute<code>sudo apt-get install postgresql libpq-dev</code>
it starts the service immediately and thus sets the locale to Latin1 (at least for the precise32 box).
Therefore, it's best practice to run
<code>sudo /usr/sbin/update-locale LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8</code>
before even installing Postgres. Note, you'll need to run <code>vagrant provision </code> to
see the locale change.
</p>

<p>
Another solution, regardless of using Vagrant or not, is to update the template of
your database instance. This can be done by running the following:

```
sudo su postgres
psql

```

```
update pg_database set datistemplate=false where datname='template1';
drop database Template1;
create database template1 with owner=postgres encoding='UTF-8'
lc_collate='en_US.utf8' lc_ctype='en_US.utf8' template template0;
update pg_database set datistemplate=true where datname='template1';
```
</p>

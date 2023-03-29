Databases
=========

The MIT Supercloud allows users to launch their own databases through
the database portal. The portal is located at:

<https://txe1-portal.mit.edu/db/dbstatus.php>

This page requires you to `authenticate into the
portal <web_portal>`{.interpreted-text role="ref"}. From here you will
see all the databases you have access to. There are Accumulo and
Postgress databases available.

To start up a database instance, press \"Start\". You can stop it by
clicking on the \"Stop\" button, and can checkpoint a stopped database
as well. Clicking on \"View Info\" for Accumulo databases will take you
to the Accumulo Monitoring page, where you can view ingest/query plots
and current tables.

If you need an instance created and cannot use one of those already
available, [contact us](https://supercloud.mit.edu/contact). Let us know
the type of database you need, what you are using it for, what it should
be called, and who should have access to the database. Let us know if
there are any special configurations you need.

There are many ways to insert and query data. While this cannot be done
from the portal page, you can query and insert data from any of the
nodes on the MIT Supercloud system. We reccommed using D4M, which has
been installed and pre-configured to work on our system with very little
effort. For more information about databases and how to use them with
D4M, take a look at the Advanced Database Technologies course on our
[online course](https://supercloud.mit.edu/online-courses) platform.
This course contains a good introduction on how to set up a data
pipeline, including parsing, ingesting, and querying data for Accumulo.

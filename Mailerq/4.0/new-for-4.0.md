# What is new in version 4.0?

MailerQ version 4.0 has some pretty big changes compared to the previous
3.0 branche. There are of course new features and bug fixes, but from a 
technical standpoint a couple of pretty  fundamental changes have been 
made.


## Performance

One of the main performance bottlenecks with the 3.0 version was the
CPU utilization of the master thread. MailerQ 4.0 therefore has a whole
different architecture.

Internally, MailerQ starts many different threads in which it does SMTP 
traffic, communication with RabbitMQ, disk IO for log files, and so 
on. These threads need to synchronize once in a while to exchange 
data, and to access the application wide counters (for example to track 
the number of deliveries per domain, the latest errors, etc). These 
global settings are managed by the master thread.

In MailerQ 3.0 the majority of all CPU time was spent by this master
thread, and other threads spent a lot of time waiting for the
master thread to get synchronized. We've changed the architecture
of MailerQ so that worker threads do not depend that much on the master
thread, more data can be accessed in non-locking way, and worker
threads can make more miles without having to synchronize.

Besides that, we've brought down the amount of data that is kept in
main memory. Most of the counters inside MailerQ were only used to display 
fancy charts on the management console, and were not used to deliver
mail. These counters have been removed (which also means that the management
console has lost a couple of charts).


## Incompatibilities

MailerQ 4.0 is not completely compatible with version 3.0. The config file has
different settings: some settings are no longer supported, others have
been added, and the values of some settings are interpreted differently.

The JSON data that you inject in MailerQ is not fully compatible with
the old data format, although it has not changed significantly. If you're 
not using anything fancy, but just the "envelope", "recipient" and "mime"
properties, there is nothing to worry about. The results that are published
back to the result queues on the other hand, have changed. The JSON 
result objects contain more data than they did before.

The tables in the SQL database have changed too. More data is kept
in the database, and less settings come from the config file (which allows 
you to make more changed on the fly without having to restart MailerQ). 
Some tables are no longer supported because the data from multiple 
tables have been merged into a single table.


## RabbitMQ connection

The rabbit MQ connection in the config file now uses a single "amqp-address"
setting. Instead of seperately setting the username, password, hostname and 
vhost in the config file, you can now assign a single "amqp://user:password@hostname/vhost" 
address to the "amqp-address" variable.


## Storage engine

The format for storage addresses has slightly changed. MongoDB
connections now use a "mongodb://" prefix, and also the address for
couchbase connection has changed a little (to couchbase://password@hostname/bucket?options).

MailerQ has been updated to use the very latest versions of the MongoDB 
and Couchbase client libraries. For MongoDB we had to switch to a complete 
new client library because the MongoDB guys stopped their work on the old 
"mongoc" driver. If you want to connect with MongoDB, make sure you have the latest version
of the [MongoDB C driver](https://github.com/mongodb/mongo-c-driver) on 
your system.

Many NoSQL client libraries sadly only offer a blocking API. 
To prevent that storage operations block the delivery of mailings, MailerQ 
therefore starts up seperate threads in which such operations are executed. 
Possible hickups in fetching or storing mime data do therefore not interfere with mail
deliveries. MailerQ 4.0 has a special config file option that you can use
to set the number of threads for these storage operations. 
If you notice a lot of hickups in storage operations, you can increment this value.

Besides these traditional NoSQL engines, we now also support the 
"dir://" storage engine. This is a very simple storage engine that stores
all data in files in a directory. This is ideal for testing purposes!


## DNS lookups

The previous versions of MailerQ used it's own non-blocking DNS resolver
for DNS lookups. A consequence of this setup was that the system wide
resolver settings (like settings found in the "etc/resolv.conf and 
"/etc/gai.conf" files) were not (always) respected. You could thus end up 
in a situation where MailerQ used different DNS data and IP addresses than 
you would expect after changing your system-wide resolver settings.

To fix this, we've removed MailerQ's own non-blocking resolver, and now
use the normal resolver functions from the system wide C library. The downside
of this approach is that DNS lookups can now be blocking again. To solve
this, MailerQ starts up a number of threads for DNS lookups to prevent that 
slow DNS servers interrupt mail deliveries. The number of threads to start 
is a config file option.

Other DNS related options in the config file are no longer supported.
These settings can now be set in the system wide "/etc/resolv.conf" setting.

MailerQ 3.0 used a helo-file in which you could list all HELO/EHLO names
for the local IP addresses. This file is no longer used, the EHLO/EHLO names
can now be managed via the management console.


## Spool directories

MailerQ can be configured to use a spool directory. This spool directory
is being monitored by MailerQ and every valid MIME formatted file that
you drop into that dir is automatically picked up, processed and gets
delivered.

This spool directory allows you to inject email more easily into MailerQ.
The old injection methods (SMTP, command line utility, and sending mail
directory to RabbitMQ) of course all still work.


## Running behind a proxy

You can now run MailerQ behind a HAProxy server. The "smtp-proxy" setting
can be used to instruct M


- smtp-proxy setting
- personalization
- incoming json update
- result json


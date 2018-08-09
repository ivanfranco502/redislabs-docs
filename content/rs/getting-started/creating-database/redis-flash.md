---
Title: Quick Setup of a Redis on Flash Database
description: $description
weight: $weight
alwaysopen: false
---
The steps to set up a Redis Enterprise Software cluster using [Redis on
Flash](/redis-enterprise-documentation/concepts-architecture/memory-architecture/redis-enterprise-flash/)
with a single node are simple and are as follows:

-   Step 1: Install Redis Enterprise Software or launch with Docker
    container
-   Step 2: Setup a Redis Enterprise Software cluster with Redis on
    Flash
-   Step 3: Create a new Redis on Flash database
-   Step 4: Connect to your new database

If you are looking for more detailed installation instructions you can
visit the [installing and upgrading
section](/redis-enterprise-documentation/administering/installing-upgrading/downloading-installing/)
of the technical documentation.

Step 1: Install Redis Enterprise Software {#step1}
-----------------------------------------

### Bare Metal, VM, Cloud Instance

To install on bare metal, VM, or instance; download the binaries from
the [Redis Enterprise download
site](https://app.redislabs.com/#/sign-up/software?direct=true). Once
you have the bits on a Linux based OS, you need to untar the image

``` {style="border: 2px solid #ddd; background-color: #333; color: #fff; padding: 10px; -webkit-font-smoothing: auto;"}
$ tar -vxf <downloaded tar file name>
```

Once the tar command completes, you will find a new install.sh script in
the current directory.

``` {style="border: 2px solid #ddd; background-color: #333; color: #fff; padding: 10px; -webkit-font-smoothing: auto;"}
$ sudo ./install.sh -y
```

### Docker-based installation

If you prefer, you can also simply run the Redis Enterprise Software
Docker container on Windows, macOS, and Linux

``` {style="border: 2px solid #ddd; background-color: #333; color: #fff; padding: 10px; -webkit-font-smoothing: auto;"}
$ docker run -d --cap-add sys_resource --name rp -p 8443:8443 -p 12000:12000 redislabs/redis:latest
```

Step 2: Setup a Cluster and Enable Redis on Flash {#step2}
-------------------------------------------------

Direct your browser to https://localhost:8443/ on the host machine to
see the Redis Enterprise Software web console. Simply click the
"**Setup**" button to get started.

Note: Depending on your browser, you may see a certificate error. Simply
choose "continue to the website" to get to the setup screen.

![](/wp-content/uploads/2017/04/setup_linux.png){.alignnone .size-full
.wp-image-26436 width="600" height="287"}

On the "**node configuration**" page, select the "Enable flash storage
support" checkbox and provide a cluster FQDN: "**mycluster.local**".
Then simply click the "**Next**" button.

![Enable Redis
Flash](/wp-content/uploads/2017/09/enable_redis_flash.png){.alignnone
.size-full .wp-image-29247 width="800" height="663"}

If you don't have a license key yet, click the "**Next"** button to try
the trial version of the product.\
On the next screen, set up a Cluster Administrator account using an
email for the login and a password.

![](/wp-content/uploads/2017/04/RP-SetupScreen4.jpeg){.alignnone
.size-full .wp-image-26409 width="600" height="377"}

Step 3: Create a Database {#step3}
-------------------------

Select the "new redis db flash" option.

![](https://redislabs.bluecoastweb.com/wp-content/uploads/2017/09/redis-on-flash.png){.alignnone
.wp-image-31433 .size-full width="600" height="613"
sizes="(max-width: 600px) 100vw, 600px"
srcset="https://redislabs.com/wp-content/uploads/2017/09/redis-on-flash.png 600w, https://redislabs.com/wp-content/uploads/2017/09/redis-on-flash-147x150.png 147w, https://redislabs.com/wp-content/uploads/2017/09/redis-on-flash-294x300.png 294w"}

On the "**new redis on flash db**" page, click the "**show advanced
option**" link and enter "**myredisflashdb**" for a database name and
"**12000**" for the endpoint port number. Then click "**Activate**" to
create your database.

![new redis flash
db](https://redislabs.com/wp-content/uploads/2017/09/newredisflashdb.png){.alignnone
.size-full .wp-image-29406 width="700" height="714"}

You now have a Redis Enterprise Flash database!

Step 4: Connecting to your Database {#step4}
-----------------------------------

With the Redis database created, you are ready to connect to your
database to store data. You can use one of the following ways to test
connectivity to your database:

-   Connecting with redis-cli, the built-in command-line tool
-   Connecting with a "hello world" application using Python

### Connecting Using redis-cli

Run redis-cli, located in the /opt/redislabs/bin directory, to connect
to port 12000 and store and retrieve a key in database1

``` {style="border: 2px solid #ddd; background-color: #333; color: #fff; padding: 10px; -webkit-font-smoothing: auto;"}
$ sudo /opt/redislabs/bin/redis-cli -p 12000
127.0.0.1:16653> set key1 123
OK
127.0.0.1:16653> get key1
"123"
```

### Connect with a simple Python app

A simple python application running in the host machine can also connect
to the database1.

Note: The following section assumes you already have python and redis-py
(python library for connecting to Redis) configured on the host machine
running the container. You can find the instructions to configure
redis-py on the [github page for
redis-py](https://github.com/andymccurdy/redis-py).

In the command-line Terminal, create a new file called
"**redis\_test.py**"

``` {style="border: 2px solid #ddd; background-color: #333; color: #fff; padding: 10px; -webkit-font-smoothing: auto;"}
$ vi redis_test.py
```

Paste the following into a file named "**redis\_test.py**".

``` {style="border: 2px solid #ddd; background-color: #333; color: #fff; padding: 10px; -webkit-font-smoothing: auto;"}
import redis

r = redis.StrictRedis(host='localhost', port=12000, db=0)
print ("set key1 123")
print (r.set('key1', '123'))
print ("get key1")
print(r.get('key1'))
```

Run "redis\_test.py" application to connect to the database and store
and retrieve a key using the command-line.

``` {style="border: 2px solid #ddd; background-color: #333; color: #fff; padding: 10px; -webkit-font-smoothing: auto;"}
$ python redis_test.py
```

The output should look like the following screen if the connection is
successful.

``` {style="border: 2px solid #ddd; background-color: #333; color: #fff; padding: 10px; -webkit-font-smoothing: auto;"}
set key1 123
True
get key1
b'123'
```

Now that you have a database, if you'd like to generate load against the
database or add a bunch of data for cluster testing,
the [memtier\_benchmark Quick
Start](/redis-enterprise-documentation/getting-started/memtier_benchmark/) should
help. However please note that to see the true performance and scale of
Redis on Flash, you will need to tune your IO path and have the flash
path set to the mounted path of SSD or NVMe flash memory as that is what
it is designed to run on. For more information, see [Redis on
Flash](/redis-enterprise-documentation/concepts-architecture/memory-architecture/redis-enterprise-flash/).
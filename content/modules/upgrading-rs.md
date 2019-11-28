---
Title: Upgrading a Module in Redis Enterprise Software
description:
weight: 3
alwaysopen: false
categories: ["Modules"]
aliases:
  - /modules/upgrading/rs
  - /rs/developing/modules/upgrading/
---

Note: Modules are not supported in Redis Enterprise Software on RHEL/CentOS 6.x

As modules are upgraded, you need to load them into Redis
Enterprise to get the new features and/or
fixes.

## Acquiring the Packaged Modules

1. Redis Enterprise pre-packaged modules - To download the upgrades
    to the modules, go to the [Redis Labs Download Center](https://redislabs.com/download-center/modules/).
1. Custom packaged modules - For instructions on packing up any [Redis module](https://redislabs.com/community/redis-modules-hub/)
    to use in upgrades, see [Developing with Modules]({{< relref "/modules/_index.md" >}}).

## Deploying the Packaged Module into Redis Enterprise Software

Once you have the upgraded package, you are ready to deploy
it:

1. Go to the **settings** tab of the
    Redis Enterprise web UI.
1. Click on **redise** **modules**
1. Click on **Add Module**

    ![upgrade_module-1](/images/rs/upgrade_module-1.png?width=1600&height=956)

1. Select the packaged module from your file system and upload
    it.
1. Go to the **databases** tab, then to
    the configuration section
1. You see in the page that an update is
    available.

    ![update_available-1](/images/rs/update_available-1.png?width=1346&height=1600)

1. At this time, you cannot upgrade this database from the web UI. It
    must be completed using the rladmin command line utility from one of
    the nodes in the cluster.

## Upgrading the Database to Use the New Version

1. SSH into any node of the cluster
1. Identify the database you are
    upgrading.

    ![rladmin_status-1](/images/rs/rladmin_status-1.png?width=1000&height=214)

1. Run the rladmin command

    `$ rladmin upgrade module db_name
    <your_db_name> module_name <module_name> version
    <new_module_version_num> module_args <module
    arguments>
    `

    Note: When this is done, it
    restarts the database shards and thus causes downtime for this
    database across the cluster.

## Examples

An example of upgrading the version of RediSearch to 10017 would
be:
```
$ rladmin upgrade module db_name MyAwesomeDB module_name ft version 10017 module_args "PARTITIONS AUTO"
```
An example of upgrading RedisBloom:
```
$ rladmin upgrade module db_name MyDB module_name bf version 10100 module_args ""
```
An example of upgrading RedisJSON:
```
$ rladmin upgrade module db_name MyDB module_name ReJSON version 10002 module_args ""
```

Each module package is a zip file. Inside the zip file is a JSON file
and it contains the information necessary for the above rladmin
command for the module_name and version information necessary. The
specific data points must be entered exactly as you see it in that JSON
file. The necessary data should be at the end of the JSON document. For
example, here is the information for the RediSearch module
that i used for the example command above:

![module_info-1](/images/rs/module_info-1.png?width=1000&height=382)
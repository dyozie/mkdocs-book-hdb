---
title: Installing MADlib  
---

You can use MADlib, an open source library for scalable in-database analytics, with your HAWQ installation.

MADlib provides a suite of SQL-based algorithms for machine learning, data mining, and statistics. These algorithms run at scale within your HAWQ database engine, eliminating any need to transfer data between HAWQ and other tools.

For additional information on MADlib, refer to the [Apache MADlib (Incubating)](http://madlib.incubator.apache.org/) project page.


## Prerequisites <a id="plrprereq"></a>

Before you install the MADlib extension for HAWQ, ensure that your HAWQ cluster is up and running.

The MADlib [Installation Guide](https://cwiki.apache.org/confluence/display/MADLIB/Installation+Guide) has additional details on the MADlib install process.

## Installing MADlib <a id="installmadlib"></a>

Installing the MADlib extension for HAWQ involves installing the MADlib package in your HAWQ cluster and registering MADlib support in any databases in which you want to use MADlib analytic functions.

### Install the MADlib Package<a id="installmadlibpkg"></a>
The MADlib extension for HAWQ is available in `.gppkg` format from [Pivotal Network](https://network.pivotal.io/products/pivotal-hdb).

Perform the following steps to install MADlib in your HAWQ cluster:

1. Download the MADlib 1.10 extension package from [Pivotal Network](https://network.pivotal.io/products/pivotal-hdb) and copy to your HAWQ master node. Make note of the directory to which the package was downloaded.

2. Log in to the HAWQ master node and set up your environment:

	``` shell
	$ ssh gpadmin@master
	gpadmin@master$ source /usr/local/hawq/greenplum_path.sh
	```

3. Uncompress and install the MADlib package:

	``` shell
	gpadmin@master$ tar xzf <madlib-pkg-download-dir>/madlib-ossv1.10.0_pv1.9.7_hawq2.2-rhel5-x86_64.tar.gz
	gpadmin@master$ gppkg -i madlib-ossv1.10.0_pv1.9.7_hawq2.2-rhel5-x86_64.gppkg
	```
	
	`gppkg` installs version 1.10 of the HAWQ MADlib extension to the `$GPHOME/madlib/` directory on all hosts in your HAWQ cluster.

 
### Register MADlib<a id="enablemadlibdb"></a>

You must explicitly register MADlib support for a HAWQ database with the `madpack` command. `madpack` is located in `$GPHOME/madlib/bin`.

`madpack` command syntax for HAWQ:

``` pre
madpack install [-s <schema-name>] -s madlib -p hawq -c <user>/<password>@<master>:<port>/<database-name>
```

The default MADlib install schema is named `madlib`. You can optionally specify an alternate schema name.

You can set the `PGUSER`, `PGPASSWORD`, `PGHOST`, `PGPORT`, and `PGDATABASE` environment variables to provide values for the connection string components in the `-c` option. If not specified on the command line or through a `PG` environment variable, MADlib prompts for the password and uses default values for the other connection string settings. 

Run `madpack --help` for detailed command and connection string syntax information.

Perform the following steps to register MADlib for a database:

1. Log in to the HAWQ master node and set up your environment:

	``` shell
	$ ssh gpadmin@master
	gpadmin@master$ source /usr/local/hawq/greenplum_path.sh
	```

1. Identify the schema in which to install MADlib objects. The default MADlib schema is named `madlib`.

2. Register MADlib support in the database:

    ``` shell
    gpadmin@master$ $GPHOME/madlib/bin/madpack install -s madlib -p hawq -c gpadmin@master:5432/<database-name>
    ```
    
    MADlib support is registered in the database in the MADlib schema named `madlib`.
    
3. Verify that MADlib was successfully registered in the database. (This command may take some time to complete.)

    ``` shell
    gpadmin@master$ $GPHOME/madlib/bin/madpack install-check -s madlib -p hawq -c gpadmin@master:5432/<database-name>
    ```
    
    When invoked with the `install-check` option, `madpack` runs some MADlib tests on the database.
    
3. Assign the appropriate users privileges to the MADlib install schema in \<database-name\>.


## Upgrading MADlib <a id="upgrademadlib"></a>

Upgrading MADlib involves upgrading the MADlib package and upgrading MADlib support in every database in which it was registered.


### Upgrade the MADlib Package<a id="upgrademadlibpkg"></a>

Perform the following steps to upgrade MADlib in your HAWQ cluster:

1. Log in to the HAWQ master node and set up your environment:

	``` shell
	$ ssh gpadmin@master
	gpadmin@master$ source /usr/local/hawq/greenplum_path.sh
	```

3. Upgrade the MADlib package:

	``` shell
	gpadmin@master$ gppkg -u madlib-ossv1.10.0_pv1.9.7_hawq2.2-rhel5-x86_64.gppkg
	```
	
	This `gppkg` command upgrades the HAWQ MADlib extension on all hosts in your HAWQ cluster to MADlib version 1.10.

 
### Upgrade MADlib Database Registration<a id="upgrademadlibdb"></a>

Perform the following steps to upgrade MADlib support in a database in which MADlib was previously registered:

1. Log in to the HAWQ master node and set up your environment:

	``` shell
	$ ssh gpadmin@master
	gpadmin@master$ source /usr/local/hawq/greenplum_path.sh
	```

1. Identify the schema in which the MADlib objects were originally registered. The default MADlib schema is named `madlib`.

2. Upgrade MADlib support in the database:

    ``` shell
    gpadmin@master$ $GPHOME/madlib/bin/madpack upgrade -s madlib -p hawq -c gpadmin@master:5432/<database-name>
    ```
    
    MADlib support is upgraded in the database in the MADlib schema named `madlib`.


## Uninstalling MADlib <a id="uninstallmadlib"></a>

Uninstalling MADlib involves removing MADlib support from any databases in which it was registered, and then removing the MADlib package from your HAWQ cluster.

### Remove MADlib Registration<a id="removemadlibdb"></a>

Use the `madpack uninstall` command to remove MADlib objects from a HAWQ database in which they were previously registered. When you remove MADlib support from a database, any functions that you created that use MADlib functionality will no longer work.

1. Log in to the HAWQ master node and set up your environment:

	``` shell
	$ ssh gpadmin@master
	gpadmin@master$ source /usr/local/hawq/greenplum_path.sh
	```

2. Remove MADlib from a database:

    ``` shell
    gpadmin@master$ madpack uninstall -s madlib -p hawq -c gpadmin@master:5432/<database-name>
    ```
    
    MADlib support is removed from the MADlib schema named `madlib`.

### Uninstall MADlib Package<a id="removemadlibppkg"></a>

Perform the following steps to uninstall the MADlib extension from your HAWQ cluster:

1. Log in to the HAWQ master node and set up your environment:

	``` shell
	$ ssh gpadmin@master
	gpadmin@master$ source /usr/local/hawq/greenplum_path.sh
	```

2. Uninstall the MADlib package:

	``` shell
	gpadmin@master$ gppkg -r madlib-ossv1.10.0_pv1.9.7_hawq2.2
	```
	
	`gppkg` uninstalls MADlib from all hosts in your HAWQ cluster.

3. Restart your HAWQ cluster:

    ``` shell
    gpadmin@master$ hawq restart cluster
    ```

    You must restart your HAWQ cluster after removing the MADlib extension package.

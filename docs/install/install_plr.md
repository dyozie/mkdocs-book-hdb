---
title: Installing PL/R
---

PL/R is available as a package that you can download from the Pivotal Network and install using the Linux `rpm` package utility. 

<p class="note"><b>Note:</b> For details on how to use PL/R, see <a href="../../hawq/plext/using_plr.html">Using PL/R</a>.</p>


## Prerequisites <a id="plrprereq"></a>

Before you install the PL/R software extension, ensure:

  - Your HAWQ cluster is up and running.
  - You have set up the HDB 2.x and HDB 2.x Add-On repositories.
  - You have installed the EPEL (Extra Packages for Enterprise Linux) repository on *each* node in your HAWQ cluster. This repository provides the required R packages and dependencies:
  
    ``` shell
    root@hawq-node$ yum install epel-release
    ```

## Install PL/R <a id="installplr"></a>

The PL/R package RPM file is available from the HDB Add-On repository.

1. On *each* HAWQ node in your cluster, install the PL/R package by running the following command:

	``` shell
	root@hawq-node$ yum install plr-hawq -y
	```

2. Log in to the HAWQ master node and set up the HAWQ environment:

    ``` shell
    $ ssh gpadmin@<master>
    gpadmin@master$ source /usr/local/hawq/greenplum_path.sh
    ```

2. Restart your HAWQ cluster:

    ``` shell
    gpadmin@master$ hawq restart cluster
    ```
 
 
## Enable PL/R Language Support <a id="enableplr"></a>

For each database that requires its use, register the PL/R language with the `CREATE LANGUAGE` SQL command.

As the `gpadmin` user, connect to the database to which you wish to add PL/R language support. For example, to add support to a database named `testdb`:

``` shell
gpadmin@master$ psql -d testdb
```
At the `psql` prompt, run the `CREATE LANGUAGE` SQL command to add PL/R support:

``` sql
testdb=# CREATE LANGUAGE plr;
```

**Note**: If you are unable to create the `plr` language due to a missing library error, you may need to update the `greenplum_path.sh` `LD_LIBRARY_PATH` setting to include `/usr/lib64/R/lib` and restart your HAWQ cluster.

You are now ready to create new or add existing PL/R functions.

### Install PL/R Functions <a id="installplrfunctions"></a>

Once PL/R is installed, HAWQ nodes have access to a library of PL/R convenience functions.  These are located in the `/usr/local/hawq/share/postgresql/contrib/plr.sql` file. 

Install these convenience PL/R functions using the `psql` utility as follows:

``` shell
gpadmin@master$ psql -d <dbname> -f /usr/local/hawq/share/postgresql/contrib/plr.sql
```
Replace \<dbname\> with the name of the target database.

**Note**: This script also adds PL/R language support to the database.

## Downloading and Installing R Packages <a id="downloadinstallplrlibries"></a>

R packages are modules that contain R functions and data sets. You can install R packages to extend R and PL/R functionality in HAWQ.

See [Downloading and Installing R Packages](../../hawq/plext/using_plr.html#downloadinstallplrlibraries)

**Note**: If you expand HAWQ and add segment hosts, you must install R, the PL/R rpm, and the R packages in the R installation of *each* of the new hosts.</p>


## Uninstall PL/R <a id="uninstallplr"></a>


When you remove PL/R language support from a database, the PL/R routines that you created in the database will no longer work.

### Remove PL/R Support for a Database <a id="removeplr"></a>


For a database that no longer requires the PL/R language, remove support for PL/R with the SQL command `DROP LANGUAGE`. 

As the `gpadmin` user, connect to the database from which you wish to remove PL/R language support. For example:

``` shell
gpadmin@master$ psql -d testdb
```

At the `psql` prompt, run the `DROP LANGUAGE` SQL command to remove PL/R support.  If you have installed the PL/R convenience functions included in `plr.sql` on the database you must use CASCADE option.

``` sql
testdb=# DROP LANGUAGE plr;
```

or

``` sql
testdb=# DROP LANGUAGE plr CASCADE;
```

### Uninstall PL/R RPM <a id="removeplrpackage"></a>

Every database must unregister PL/R support before you can uninstall the PL/R extension package. 

To uninstall the PL/R package, run the following command on *each* node in your HAWQ cluster:

``` shell
root@hawq-node$ yum erase plr-hawq
```

Finally, log in to the HAWQ master node and restart the cluster.

``` shell
gpadmin@master$ hawq restart cluster
```


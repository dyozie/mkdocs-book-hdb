---
title: Pivotal HDB 2.2.0 Release Notes
---

Pivotal HDB 2.2.0 is a minor release of the product and is based on [Apache HAWQ \(Incubating\)](http://hawq.incubator.apache.org/). This release includes RHEL/CentOS 7 support, Beta support for Apache Ranger integration, and bug fixes.

## Supported Platforms <a id="topic_dhh_2jx_yt"></a>

The supported platform for running Pivotal HDB 2.2.0 comprises:

-   Red Hat Enterprise Linux \(RHEL\) 6.4+, 7.2+ \(64-bit\) \(See note in [Known Issues and Limitations](#issues-os) for kernel limitations.\)
-   CentOS 7
-   [Hortonworks Data Platform \(HDP\) 2.5.3](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.3/bk_release-notes/content/ch_relnotes_v253.html).
-   [Ambari 2.4.2](http://docs.hortonworks.com/HDPDocuments/Ambari-2.4.2.0/bk_ambari-release-notes/content/ch_relnotes-ambari-2.4.2.0.html) \(for Ambari-based installation and HAWQ cluster management\).

Each Pivotal HDB host machine must also meet the Apache HAWQ \(Incubating\) system requirements. [See Apache HAWQ System Requirements](../../hawq/requirements/system-requirements.html) for more information.

### Product Support Matrix<a id="topic_g53_tgv_2v"></a>

The following table summarizes Pivotal HDB product support for current and previous versions of Pivotal HDB, Hadoop, HAWQ, Ambari, and operating systems.

|Pivotal HDB Version|PXF Version |HDP Version Requirement (Pivotal HDP and Hortonworks HDP)|Ambari Version Requirement|HAWQ Ambari Plug-in Requirement|MADlib Version Requirement | RHEL/CentOS Version Requirement|SuSE Version Requirement|
|-------------------|--------------|-----------------------|--------------------------|-------------------------------|-----------------------|--------|------------------------|
|2.2.0.0|3.2.1.0|2.5.3| 2.4.2 |2.2.0.0|1.9, 1.9.1, 1.10|6.4+, 7.2+ \(64-bit\)|n/a|
|2.1.2.0|3.2.0.0|2.5|2.4.1, 2.4.2 |2.1.2.0|1.9, 1.9.1, 1.10|6.4+ \(64-bit\)|n/a|
|2.1.1.0|3.1.1.0|2.5|2.4.1 |2.1.1.0|1.9, 1.9.1|6.4+ \(64-bit\)|n/a|
|2.1.0.0|3.1.0.0|2.5|2.4.1 |2.1.0.0|1.9, 1.9.1|6.4+ \(64-bit\)|n/a|
|2.0.1.0|3.0.1|2.4.0, 2.4.2|2.2.2, 2.4 |2.0.1|1.9, 1.9.1|6.4+ \(64-bit\)|n/a|
|2.0.0.0|3.0.0|2.3.4, 2.4.0|2.2.2|2.0.0|1.9, 1.9.1|6.4+ \(64-bit\)|n/a|
|1.3.1.1|2.5.1.1|2.2.6|2.0.x|1.3.1|1.7.1, 1.8, 1.9, 1.9.1|6.4+|SLES 11 SP3|
|1.3.1.0|2.5.1.1|2.2.6|2.0.x|1.3.1|1.7.1, 1.8, 1.9, 1.9.1|6.4+|SLES 11 SP3|
|1.3.0.3|1.3.3|2.2.4.2|1.7|1.2|1.7.1, 1.8, 1.9, 1.9.1|6.4+|SLES 11 SP3|
|1.3.0.2|1.3.3|2.2.4.2|1.7|1.2|1.7.1, 1.8, 1.9, 1.9.1|6.4+|SLES 11 SP3|
|1.3.0.1|1.3.3|2.2.4.2|1.7|1.1|1.7.1, 1.8, 1.9, 1.9.1|6.4+|n/a|
|1.3.0.0|1.3.3|n/a|n/a|n/a|1.7.1, 1.8, 1.9, 1.9.1|n/a|n/a|


### Procedural Language Support Matrix<a id="topic_plsupportmatrix"></a>

The following table summarizes component version support for Procedural Languages available in Pivotal HDB 2.x. The versions listed have been tested with Pivotal HDB. Higher versions may be compatible. Please test higher versions thoroughly in your non-production environments before deploying to production.

|Pivotal HDB Version|PL/Java Java Version Requirement|PL/R R Version Requirement|PL/Perl Perl Version Requirement|PL/Python Python Version Requirement|
|-------------------|-----------------------|--------------------------|-------------------------------|-------------------------------|
|2.2.0.0|1.7| 3.3.1 |5.10.1|2.6.2|
|2.1.x.0|1.7| 3.3.1 |5.10.1|2.6.2|
|2.0.1.0|1.7|3.3.1 |5.10.1|2.6.2|
|2.0.0.0|1.6, 1.7|3.1.0|5.10.1|2.6.2|


### AWS Support Requirements <a id="topic_ywd_4fv_2v"></a>

Pivotal HDB is supported on Amazon Web Services \(AWS\) servers using either Amazon block level Instance store \(Amazon uses the volume names ephemeral\[0-23\]\) or Amazon Elastic Block Store \(Amazon EBS\) storage. Use long-running EC2 instances with these for long-running HAWQ instances, as Spot instances can be interrupted. If using Spot instances, minimize risk of data loss by loading from and exporting to external storage.


##  Pivotal HDB 2.2.0 Features and Changes <a id="whatsnewintherelease"></a>

Pivotal HDB 2.2.0 is based on [Apache HAWQ \(Incubating\)](http://hawq.incubator.apache.org/), and includes the following new features as compared to Pivotal HDB 2.1.2:

- *Ranger Integration \(Beta\)* 

    Pivotal HDB 2.2.0 introduces Beta support for Apache Ranger. The HAWQ Ranger Plugin Service is a RESTful service that provides integration between HDB and Ranger policy management.  Ranger integration enables you to use Apache Ranger to authorize user access to HAWQ resources. Using Ranger enables you to manage all of your Hadoop components’ authorization policies using the same user interface, policy store, and auditing stores. Refer to the [Overview of Ranger Policy Management](../../hawq/ranger/ranger-overview.html) for specific information on HAWQ integration with Ranger.

- *RHEL/CentOS 7.2+ support*

    Pivotal HDB 2.2.0 now provides product downloads for Red Hat Enterprise Linux 7 and CentOS 7 operating systems.
    
- *PXF ORC with Pivotal HDB*

    Pivotal HDB 2.2.0 now fully supports PXF with Optimized Row Columnar \(ORC\) file format, which was formerly BETA.


## Pivotal HDB 2.2.0 Upgrade <a id="hdb20121xmigrate"></a>

Pivotal HDB 2.2.0 upgrade paths:

- The [**Upgrading from Pivotal HDB 2.1.x**](../install/HDB21xto22xUpgrade.html) guide provides specific details on applying the Pivotal HDB 2.2.0 minor release to your HDB 2.1.0, 2.1.1, or 2.1.2 installation.

- The [**Upgrading from Pivotal HDB 2.0.1**](../install/HDB20xto22xUpgrade.html) guide details the steps involved to upgrade your Pivotal HDB 2.0.1 installation to HDB 2.2.0.

**Note**: There is no direct upgrade path from Pivotal HDB 1.x to HDB 2.2.0. For more information on considerations for upgrading from Pivotal 1.x releases to Pivotal 2.x releases, refer to the [Pivotal HDB 2.0 documentation](http://hdb.docs.pivotal.io/200/hdb/releasenotes/HAWQ20ReleaseNotes.html#upgradepaths). Contact your Pivotal representative for assistance in migrating from HDB 1.x to HDB 2.x.

## Differences Compared to Apache HAWQ \(Incubating\)<a id="haqhdbdiffs"></a>


Pivotal HDB 2.2.0 does not currently support the PXF JDBC Plug-in.

Otherwise, Pivotal HDB 2.2.0 includes all of the functionality in [Apache HAWQ \(Incubating\)](http://hawq.incubator.apache.org/).

## Resolved Issues<a id="resolvedissues"></a>
The following HAWQ and PXF issues were resolved in Pivotal HDB 2.2.0.

| Apache Jira | Component | Summary |
| ----------- | --------- | ------- |
| [HAWQ-256](https://issues.apache.org/jira/browse/HAWQ-256) | Security | Integrated Security with Apache Ranger (Partial beta-release implementation) |
| [HAWQ-309](https://issues.apache.org/jira/browse/HAWQ-309) | Build | Support for CentOS/RHEL 7 |
| [HAWQ-944](https://issues.apache.org/jira/browse/HAWQ-944) | Core | Numutils.c: `pg_ltoa` and `pg_itoa` functions allocated unnecessary amounts of bytes |
| [HAWQ-1063](https://issues.apache.org/jira/browse/HAWQ-1063) | Command Line Tools | Fixed HAWQ Python library missing import |
| [HAWQ-1314](https://issues.apache.org/jira/browse/HAWQ-1314) | Catalog, Hcatalog, PXF | The pxf\_get\_item\_fields function would stop working after upgrade |
| [HAWQ-1347](https://issues.apache.org/jira/browse/HAWQ-1347) | Dispatcher | QD should only check segment health  |
| [HAWQ-1365](https://issues.apache.org/jira/browse/HAWQ-1365) | Core  | Print out detailed schema information for tables, even if the user doesn't have privileges access privileges|
| [HAWQ-1366](https://issues.apache.org/jira/browse/HAWQ-1366) | Storage | HAWQ should throw an error if it finds a dictionary encoding type for Parquet |
| [HAWQ-1371](https://issues.apache.org/jira/browse/HAWQ-1371) | Query Execution | QE process hang in shared input scan |
| [HAWQ-1378](https://issues.apache.org/jira/browse/HAWQ-1378) | Core | Elaborate the "invalid command-line arguments for server process" error |
| [HAWQ-1379](https://issues.apache.org/jira/browse/HAWQ-1379) | Core | Do not send options multiple times in build\_startup\_packet |
| [HAWQ-1385](https://issues.apache.org/jira/browse/HAWQ-1385) | Command Line Tools | `hawq_ctl stop` fails when the master is down |
| [HAWQ-1408](https://issues.apache.org/jira/browse/HAWQ-1408) | Core | COPY ... FROM STDIN causes PANIC |
| [HAWQ-1418](https://issues.apache.org/jira/browse/HAWQ-1418) | Command Line Tools | Print executing command for hawq register |

## Known Issues and Limitations <a id="knownissues"></a>


### MADlib 1.9.x Compression<a id="madlib-install"></a>
Pivotal HDB 2.2.0 is compatible with MADlib 1.9, 1.9.1, and 1.10. *If you have an existing HDB installation with MADlib 1.9.x installed, or are installing MADlib 1.9.x*, you must download and execute a script to remove MADlib's use of Quicklz compression, which is not supported in HDB 2.2.0. Run this script if you are upgrading an HDB installation with MADlib 1.9.x to HDB 2.2.0, or if you are installing MADlib 1.9.x on HDB 2.2.0.

This procedure is not necessary if you are using or installing MADlib 1.10, or if you have previously disabled Quicklz compression.

**If you are upgrading an HDB 2.0 system that contains MADlib:**

1. Complete the Pivotal HDB 2.2.0 upgrade procedure as described in [Upgrading to Pivotal HDB 2.2.0](../install/HDB20xto22xUpgrade.html).

2. Download and unpack the MADlib 1.9.x binary distribution from the [Pivotal HDB Download Page](https://network.pivotal.io/products/pivotal-hdb) on Pivotal Network.

3. Execute the `remove_compression.sh` script in the MADlib 1.9.x distribution, providing the path to your existing MADlib installation:

    ``` shell
    $ remove_compression.sh --prefix <madlib-installation-path>
    ```
   
    **Note:** If you do not include the `--prefix` option, the script uses the location `${GPHOME}/madlib`.

**For new MADlib installations, complete these steps after you install Pivotal HDB 2.2.0:**

1. Download and unpack the MADlib 1.9.x binary distribution from the [Pivotal HDB Download Page](https://network.pivotal.io/products/pivotal-hdb) on Pivotal Network.

2. Install the MADlib `.gppkg` file:

    ``` shell
    $ gppkg -i <path-to>/madlib-ossv1.9.1_pv1.9.6_hawq2.1-rhel5-x86_64.gppkg
    ```
3. Execute the `remove_compression.sh` script, optionally providing the MADlib installation path:

    ``` shell
    $ remove_compression.sh --prefix <madlib-installation-path>
    ```
   
    **Note:** If you do not include the `--prefix` option, the script uses the location `${GPHOME}/madlib`.
    
4. Continue installing MADlib using the `madpack install` command as described in the MADlib [Installation Guide](https://cwiki.apache.org/confluence/display/MADLIB/Installation+Guide). For example:

    ``` shell
    $ madpack –p hawq install
    ```

### Operating System<a id="issues-os"></a>
-   Some Linux kernel versions between 2.6.32 to 4.3.3 \(not including 2.6.32 and 4.3.3\) have a bug that could introduce a `getaddrinfo()` function hang. To avoid this issue, upgrade your RHEL-6 kernel to version 4.3.3+.
-   If you are running RHEL-7, ensure that your kernel version is 3.10.0-327.27 or above, otherwise you may experience hangs with large workloads.
  
### Ranger Integration (Beta)<a id="issues-ranger"></a>
-   Beta-level support of Ranger Plugin Service has a number of known limitations. For more information, refer to [Limitations of Ranger Policy Management](../../hawq/ranger/ranger-overview.html#limitations).


### PXF<a id="issues-pxf"></a>
-   GPSQL-3345 - To take advantage of the change in number of virtual segments, PXF external tables must be dropped and recreated after updating the `default_hash_table_bucket_number` server configuration parameter.
-   GPSQL-3347 - The `LOCATION` string provided when creating a PXF external table must use only ASCII characters to identify a file path. Specifying double-byte or multi-byte characters in a file path returns the following error (formatted for clarity):

    ``` pre
    ERROR: remote component error (500) from 'IP_Address:51200': type Exception report
      message:  File does not exist: /tmp/??????/ABC-??????-001.csv
      description:  The server encountered an internal error that prevented it from fulfilling this request.
      exception:  java.io.IOException: File does not exist: /tmp/??????/ABC-??????-001.csv (libchurl.c:897) (seg10 hdw2.hdp.local:40000 pid=389911) (dispatcher.c:1801)
    ```
-   PXF in a Kerberos-secured cluster requires YARN to be installed due to a dependency on YARN libraries.
-   In order for PXF to interoperate with HBase, you must manually add the PXF HBase JAR file to the HBase classpath after installation. See [Post-Install Procedure for Hive and HBase on HDP](../install/install-ambari.html#post-install-pxf).
-   [HAWQ-974](https://issues.apache.org/jira/browse/HAWQ-974) - When using certain PXF profiles to query against larger files stored in HDFS, users may occasionally experience hanging or query timeout. This is a known issue that will be improved in a future HDB release.  Refer to [Addressing PXF Memory Issues](../../hawq/pxf/TroubleshootingPXF.html#pxf-memcfg) for a discussion of the configuration options available to address these issues in your PXF deployment.
-   The `HiveORC` profile supports aggregate queries (count, min, max, etc.), but they have not yet been optimized to leverage ORC file- and stripe-level metadata.
-   The `HiveORC` profile does not yet use the Vectorized Batch reader.

### PL/R<a id="issues-plr"></a>
The HAWQ PL/R extension is provided as a separate RPM in the `hdb-add-ons-2.2.0.0` repository. The files installed by this RPM are owned by `root`. If you installed HAWQ via Ambari, HAWQ files are owned by `gpadmin`. Perform the following steps on each node in your HAWQ cluster after PL/R RPM installation to align the ownership of PL/R files:

``` shell
root@hawq-node$ cd /usr/local/hawq
root@hawq-node$ chown gpadmin:gpadmin share/postgresql/contrib/plr.sql docs/contrib/README.plr lib/postgresql/plr.so
```

### Ambari<a id="issues-install"></a>

-  Ambari-managed clusters should only use Ambari for setting server configuration parameters. Parameters modified using the `hawq config`command will be overwritten on Ambari startup or reconfiguration.
-   When installing HAWQ in a Kerberos-secured cluster, the installation process may report a warning/failure in Ambari if the HAWQ configuration for resource management type is switched to YARN mode during installation. The warning is related to HAWQ not being able to register with YARN until the HDFS & YARN services are restarted with new configurations resulting from the HAWQ installation process.
-   The HAWQ standby master will not work after you change the HAWQ master port number. To enable the standby master you must first remove and then re-initialize it. See [Removing the HAWQ Standby Master](../../hawq/admin/ambari-admin.html#amb-remove-standby) and [Activating the HAWQ Standby Master](../../hawq/admin/ambari-admin.html#amb-activate-standby).
-   The Ambari **Re-Synchronize HAWQ Standby Master** service action fails if there is an active connection to the HAWQ master node. The HAWQ task output shows the error, `Active connections. Aborting shutdown...` If this occurs, close all active connections and then try the re-synchronize action again.
-   The Ambari **Run Service Check** action for HAWQ and PXF may not work properly on a secure cluster if PXF is not co-located with the YARN component.
-   In a secured cluster, if you move the YARN Resource Manager to another host you must manually update `hadoop.proxyuser.yarn.hosts` in the HDFS `core-site.xml` file to match the new Resource Manager hostname. If you do not perform this step, HAWQ segments fail to get resources from the Resource Manager.
-   The Ambari **Stop HAWQ Server (Immediate Mode)** service action or `hawq stop -M immediate` command may not stop all HAWQ master processes in some cases. Several `postgres` processes owned by the `gpadmin` user may remain active.
-   Ambari checks whether the `hawq_rm_yarn_address` and `hawq_rm_yarn_scheduler_address` values are valid when YARN HA is not enabled. In clusters that use YARN HA, these properties are not used and may get out-of-sync with the active Resource Manager. This can leading to false warnings from Ambari if you try to change the property value.
-  Ambari does not support Custom Configuration Groups with HAWQ.
- Certain HAWQ server configuration parameters related to resource enforcement are not active. Modifying the parameters has no effect in HAWQ since the resource enforcement feature is not currently supported. These parameters include `hawq_re_cgroup_hierarchy_name`, `hawq_re_cgroup_mount_point`, and `hawq_re_cpu_enable`. These parameters appear in the **Advanced hawq-site** configuration section of the Ambari management interface.

#### Workaround Required after Moving Namenode<a id="nn-workaround"></a>
If you use the Ambari Move Namenode Wizard to move a Hadoop namenode, the Wizard does not automatically update the HAWQ configuration to reflect the change. This leaves HAWQ in an non-functional state, and will cause HAWQ service checks to fail with an error similar to:

<pre><code>
2017-04-19 21:22:59,138 - SQL command executed failed: export PGPORT=5432 && source
/usr/local/hawq/greenplum_path.sh && psql -d template1 -c \\\\\"CREATE  TABLE
ambari_hawq_test (col1 int) DISTRIBUTED RANDOMLY;\\\\\"
Returncode: 1
Stdout:
Stderr: Warning: Permanently added 'ip-10-32-36-168.ore1.vpc.pivotal.io,10.32.36.168'
(RSA) to the list of known hosts.
WARNING:  could not remove relation directory 16385/1/18366: Input/output error
CONTEXT:  Dropping file-system object -- Relation Directory: '16385/1/18366'
ERROR:  could not create relation directory
hdfs://ip-10-32-36-168.ore1.vpc.pivotal.io:8020/hawq_default/16385/1/18366: Input/output error
<br/>
2016-04-19 21:22:59,139 - SERVICE CHECK FAILED: HAWQ was not able to write and query
from a table
2016-04-19 21:23:02,608 - ** FAILURE **: Service check failed 1 of 3 checks
stdout:   /var/lib/ambari-agent/data/output-281.txt
</code></pre>

To work around this problem, perform one of the following procedures after you complete the Move Namenode Wizard.

##### Workaround for Non-HA NameNode Clusters:

1. Perform an HDFS service check to ensure that HDFS is running properly after you moved the NameNode.
2. Use the Ambari `config.sh` utility to update `hawq_dfs_url` to the new NameNode address. See the [Modify configurations](https://cwiki.apache.org/confluence/display/AMBARI/Modify+configurations) on the Ambari Wiki for more information. For example:

    ``` shell
    $ cd /var/lib/ambari-server/resources/scripts/
    $ ./configs.sh set {ambari_server_host} {clustername} hawq-site
    $ hawq_dfs_url {new_namenode_address}:{port}/hawq_default
    ```
    
3. Restart the HAWQ configuration to apply the configuration change.
4. Use `ssh` to log into a HAWQ node and run the `checkpoint` command:

    ``` shell
    $ psql -d template1 -c "checkpoint"
    ```
6. Stop the HAWQ service.
7. The master data directory is identified in the `$GPHOME/etc/hawq-site.xml` file `hawq_master_directory` property value. Copy the master data directory to a backup location:

    ``` shell
    $ export MDATA_DIR=/value/from/hawqsite
    $ cp -r $MDATA_DIR /catalog/backup/location
    ```
8. Execute this query to display all available HAWQ filespaces:
9. 
    ``` sql
    SELECT fsname, fsedbid, fselocation FROM pg_filespace AS sp,
pg_filespace_entry AS entry, pg_filesystem AS fs WHERE sp.fsfsys = fs.oid
AND fs.fsysname = 'hdfs' AND sp.oid = entry.fsefsoid ORDER BY
entry.fsedbid;
    ```
    ``` pre
          fsname | fsedbid | fselocation
-------------+---------+------------------------------------------------
cdbfast_fs_a | 0       | hdfs://hdfs-cluster/hawq//cdbfast_fs_a
dfs_system   | 0       | hdfs://test5:9000/hawq/hawq-1459499690
(2 rows)
    ```
9. Execute the `hawq filespace` command on each filespace that was returned by the previous query. For example:

    ``` shell
    $ hawq filespace --movefilespace dfs_system --location=hdfs://new_namenode:port/hawq/hawq-1459499690
    $ hawq filespace --movefilespace cdbfast_fs_a --location=hdfs://new_namenode:port/hawq//cdbfast_fs_a
    ```
9. If your cluster uses a HAWQ standby master, reinitialize the standby master in Ambari using the Remove Standby Wizard followed by the Add Standby Wizard.
9. Start the HAWQ Service.
9. Run a HAWQ service check to ensure that all tests pass.

##### Workaround for HA NameNode Clusters:

1. Perform an HDFS service check to ensure that HDFS is running properly after you moved the NameNode.
2. Use Ambari to expand `Custom hdfs-client` in the HAWQ Configs tab, then update the `dfs.namenode.` properties to match the current NameNode configuration.
3. Restart the HAWQ configuration to apply the configuration change.
9. Run a HAWQ service check to ensure that all tests pass.

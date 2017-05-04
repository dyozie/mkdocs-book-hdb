---
title: HDB 2.1.x to HDB 2.2.y Upgrade
---

The HDB 2.1.x to HDB 2.2.y upgrade procedure is described below. This procedure applies a minor update to an existing HDB 2.1 installation, upgrading Ambari, HDP, the Ambari HAWQ Plug-in, HDB, and PXF components to supported versions. For example, use this procedure to upgrade from HDB 2.1.1 to HDB 2.2.0, or to any later maintenance release version. This procedure uses HDB 2.1.x to refer to your currently-installed HDB version, and HDB 2.2.y to refer to the minor release to which you are upgrading.

Pre- and post- upgrade component versions are identified in the table below.

|Component|Pre-Upgrade Version| Post-Upgrade Version |
|------|------------|-----|
|Ambari |2.4.1, 2.4.2| 2.4.2|
|HDP |2.5| 2.5.3|
|Ambari HAWQ Plug-in|2.1.0, 2.1.1, 2.1.2| 2.2.0|
|HDB|2.1.0, 2.1.1, 2.1.2| 2.2.0|
|PXF|3.1.0, 3.1.1, 3.2.0| 3.2.1|

This upgrade procedure **must** be performed in the exact order presented:

**Part 1: Upgrade the Stack**

-   [Step 1: Upgrade Ambari](#upgrade_ambari)
-   [Step 2: Upgrade HDP](#upgrade_hdp)

**Part 2: Upgrade HAWQ**

-   [Step 3: Add New HDB Software Repositories](#up_setupyum)
-   [Step 4: Upgrade Ambari HAWQ Plug-in](#up_ambaripluginup)
-   [Step 5: Back up Existing HDB Installation](#up_backupinst)
-   [Step 6: Upgrade HDB Software](#up_installhdb)
-   [Step 7: Upgrade PXF](#up_pxfup)
-   [Step 8: Ambari Post-Upgrade Actions for PXF](#uphawq_ambarimgd)
-   [Step 9: HDB 2.1.x Post-Upgrade Actions](#uphawq_final)

## <a id="upgrade_ambari"></a>Step 1: Upgrade Ambari

If you use Ambari to manage your cluster and your Ambari version is older than 2.4.2:

1. Download [Ambari 2.4.2](http://docs.hortonworks.com/HDPDocuments/Ambari-2.4.2.0/bk_ambari-installation/content/ambari_repositories.html).

2. Follow the instructions in [Upgrading Ambari to 2.4.2](http://docs.hortonworks.com/HDPDocuments/Ambari-2.4.2.0/bk_ambari-upgrade/content/upgrading_ambari.html) in the Hortonworks Pivotal HDP documentation.  

## <a id="upgrade_hdp"></a>Step 2: Upgrade HDP

Perform the following steps to upgrade HDP in your Ambari-managed installation:  

1. Start all services by selecting **Start All** from the Ambari service pane **Actions** drop down. 

2. Perform service checks by clicking on each service in the service pane and selecting **Run Service Check** from the **Service Actions** drop down. 

3. Navigate to HDFS service **Configs > Advanced > Custom hdfs-site** and set the `dfs.allow.truncate` property to `false`. (Alternatively, you can enter this property in the HDFS **Configs** search bar.)

    Do not yet restart HDFS and dependent services.

8. Stop the HAWQ service due to the incompatible `dfs.allow.truncate` setting. First, click on the service in the left pane and then select the **Stop** action from the **Service Action** dropdown.

9. **Restart** HDFS and dependent services. *Do not restart HAWQ*. Perform  service checks on these services.

4. Upgrade HDP to version 2.5.3. Follow the instructions in [Upgrading HDP](http://docs.hortonworks.com/HDPDocuments/Ambari-2.4.2.0/bk_ambari-upgrade/content/upgrading_hdp_stack.html) in the Hortonworks Pivotal HDP documentation.

5. Navigate back to HDFS service **Configs > Advanced > Custom hdfs-site** and set the `dfs.allow.truncate` property back to `true`. **Restart** the HDFS and any dependent services. 

6. Restart HAWQ.

7. Verify all services are running (green).


## <a id="up_setupyum"></a>Step 3: Add New HDB Software Repositories

Follow the instructions in [Setting up HDB Repositories](setup-hdb-repos.html) to set up local `yum` HDB repositories on the `repo-node` you choose to host the HDB software. This system must be accessible to all nodes in your HAWQ cluster.
   
After you set up the repos, each HAWQ host can obtain the HDB software from the `repo-node` HDB repositories.


## <a id="up_ambaripluginup"></a>Step 4: Upgrade Ambari HAWQ Plug-in

If you use Ambari to manage your cluster, perform the following steps to upgrade the Ambari HAWQ plug-in software.

**Note**: The Ambari plug-in for HAWQ is included in the HDB software installation package.

1. Log in to the Ambari server system as the `root` user:

    ``` shell
    $ ssh root@<ambari-server>
    ```

2. Install the HAWQ Ambari plug-in. The RPM package will be installed from the HDB repository set up on `repo-node` in the previous section.

    ``` shell
    root@ambari-server$ yum install hawq-ambari-plugin
    ```
    
    Scripts and template files are installed to `/var/lib/hawq`.

3. Ensure that the Ambari server is running. If not, start it:

    ```` shell
    root@ambari-server$ /usr/sbin/ambari-server status
    root@ambari-server$ /usr/sbin/ambari-server start
    ````

4. Run the `add-hawq.py` Ambari HAWQ set-up script, providing the HDB repository URL (replace `y` with the appropriate HDB maintenance version number) and HDP stack version; you will be prompted for your Ambari admin credentials if you do not provide `--user` and `--password` options:

    ``` shell
    root@ambari-server$ /var/lib/hawq/add-hawq.py  -s HDP-2.5 --hawqrepo http://<repo-node>/hdb-2.2.y.0
    ```
    
    **Note**: The `--hawqrepo` option is not required if you have set up the HDB repositories on your Ambari server host.

5. Restart the Ambari server:

    ``` shell
    root@ambari-server$ /usr/sbin/ambari-server restart
    ```

7. Stop the PXF and HAWQ services. First, click on the service in the left pane and then select the **Stop** action from the **Service Action** dropdown.

9. Unless directed to by these instructions, do not invoke any service actions in the Ambari management console until the upgrade procedure is complete.

## <a id="up_backupinst"></a>Step 5: Back up Existing HDB Installation

Backing up your HDB installation before a minor upgrade is recommended.

1. Log in to the HDB 2.1.x HAWQ master node and set up the environment. For example:

    ``` shell
    $ ssh gpadmin@<master>
    gpadmin@master$ . /usr/local/hawq/greenplum_path.sh
    gpadmin@master$ hawq version
    HAWQ version 2.1.0.0 build 2490
    ```

2. (*Optional*) Clean out old server log files from your master and segment data directories. Cleaning out old log files is not required, but will reduce the size of the HAWQ files that are backed up.
    
3. Follow the instructions in [Backing Up and Restoring HAWQ](../../hawq/admin/BackingUpandRestoringHAWQDatabases.html) to back up your existing databases.

4. Backing up the HDB 2.1.x binary installation is not necessary; HDB version 2.0.1 and newer support upgrade in place.

5. Stop the PXF Agent on *each* node if you have not done this from the Ambari UI in a previous step:

    ``` shell
    hawq-node$ sudo service pxf-service stop
    ```

5. Stop the HAWQ cluster if you have not done this from the Ambari UI in a previous step:

    ``` shell
    gpadmin@master$ hawq stop cluster
    ```

6. Back up data directories on the HAWQ master, standby master, and all segment nodes. You can determine the location of the data directories by examining the `hawq_master_directory` and `hawq_segment_directory` configuration values in `$GPHOME/etc/hawq-site.xml`.

    For example, to copy the master node data directory to the local system:

    ``` shell
    master$ mkdir -p /save/hawq-backup/data
    master$ cp -r /data/hawq/master /save/hawq-backup/data/
    ```

    On a segment node:

    ``` shell
    segment$ cp -r /data/hawq/segment /save/hawq-backup/data/
    ```

7. Copy or preserve any additional folders or files (such as backup folders) that you have added in the HAWQ data directories.

## <a id="up_installhdb"></a>Step 6: Upgrade HDB Software

Perform the following steps on the HAWQ master, HAWQ standby master, and on **each** HAWQ segment node.  You may choose to update your cluster in parallel using your choice of tool such as `ssh`.

1. Log in to the HAWQ node as the `root` user:
 
    ``` shell
    $ ssh root@<hawq-node>
    ```

2. Install the HDB software on the node:
 
    ``` shell
    root@hawq-node$ yum upgrade hawq
    ```
        
    The HAWQ software is installed to `/usr/local/hawq_2_2_y_0` (replace `y` with the appropriate HDB maintenance version number).
    
3. Create a symbolic link to the new HAWQ installation directory:

    ``` shell
    root@hawq-node$ ln -s /usr/local/hawq_2_2_y_0 /usr/local/hawq
    ```

3. If you do not use Ambari to manage your HAWQ cluster, copy the HDB 2.1.x  `etc/` configuration directory to the new HDB 2.2.y installation:

    ``` shell
    hawq-node$ cp -rf /usr/local/hawq_2_1_x_0/etc /usr/local/hawq/
    hawq-node$ chown -R gpadmin:gpadmin /usr/local/hawq
    ```

6. Perform steps 1-4. above on the HAWQ masters and *each* segment node in your HAWQ cluster.

4. Restart the HAWQ cluster:

    a. If you use Ambari to manage your cluster, start the HAWQ service from the Ambari console.

    b. If you do not use Ambari, log in to the HAWQ master node and restart the HAWQ cluster:

    ``` shell
    $ ssh gpadmin@<master>
    gpadmin@master$ . /usr/local/hawq/greenplum_path.sh
    gpadmin@master$ hawq start cluster
    ```

8. Display the HAWQ version to verify the HDB software upgrade. For example:

    ``` shell
    gpadmin@master$ hawq version
    HAWQ version 2.2.y.0 build NNNN
    ```

## <a id="up_pxfup"></a>Step 7: Upgrade PXF

Perform the following steps on **each** PXF node in your HAWQ cluster to upgrade the PXF software:

1. Log in to the PXF node as the `root` user:

    ``` shell
    $ ssh root@<pxf-node>
    ```

2. Stop the PXF service on each node if you have not done this from the Ambari UI in a previous step:

    ``` shell
    root@pxf-node$ service pxf-service stop
    ```
    
3. Back up the PXF 3.1.x configuration files found in the `/etc/pxf/conf/` directory. For example:

    ``` shell
    root@pxf-node$ mkdir -p /save/pxf31x-conf
    root@pxf-node$ cp /etc/pxf/conf/* /save/pxf31x-conf/
    ```

4. Uninstall the PXF 3.1.x RPMs:

    ``` shell
    root@pxf-node$ yum erase -y pxf*
    ```

5. Remove the PXF service instance:

    ``` shell
    root@pxf-node$ rm -rf /var/pxf /etc/pxf*
    ```

4. Upgrade the PXF software to version 3.2.y:

    ``` shell
    root@pxf-node$ yum install -y pxf
    ```
    
    This command installs the PXF agent software and all PXF plug-ins: HDFS, Hive, HBase, JSON. The PXF software is installed to `/usr/lib/pxf-3.2.y.0` and `/etc/pxf-3.2.y.0`, with links created from `/usr/lib/pxf` and `/etc/pxf` respectively.

7. If you updated any PXF configuration files in your original installation, propagate these changes to the new PXF installation. Specifically, manual changes to `pxf-env.sh` (especially the `$JAVA_HOME` setting), `pxf-profiles.xml`, `pxf-site.xml`, and `pxf-public.classpath` files will need to be ported over.
    
8. Initialize the PXF service; ensure that you have set `$JAVA_HOME` before initializing:

    ``` shell
    root@pxf-node$ service pxf-service init
    ```

8. After upgrading PXF on each node, start the PXF service:

    a. If you use Ambari to manage your cluster, start the PXF service via the Ambari console.

    b. If you do not use Ambari, start the PXF service from the command line on *each* node:

    ``` shell
    root@pxf-node$ service pxf-service start
    ```

## <a id="uphawq_ambarimgd"></a>Step 8: Ambari Post-Upgrade Actions for PXF

If you manage your HAWQ cluster with Ambari and use PXF, follow the instructions in [Ambari Post-Install Procedure for HDB 2.2.0](install-ambari.html#post-install-212-req) to manually update your Hive profile configurations.

## <a id="uphawq_final"></a>Step 9: HDB 2.1.x Post-Upgrade Actions

After you have verified your HDB 2.2.y installation was upgraded successfully and your HAWQ cluster has been running without issues, uninstall HDB 2.1.x by performing the following operation on **each** node in your HAWQ cluster:

``` shell
root@hawq-node$ yum erase hawq_2_1_x_0
```

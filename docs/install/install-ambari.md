---
title: Install Apache HAWQ using Ambari
---

## Prerequisites <a id="section_mqs_f3j_5r"></a>

-   Install a compatible version of HDP and Ambari, and ensure that your HDP system is fully functional. See the [Release Notes](../releasenotes/HAWQ220ReleaseNotes.html#topic_dhh_2jx_yt) for more information about component version compatibility.

-   Select and prepare all host machines that will run the HAWQ and PXF services. See [Apache HAWQ System Requirements](../../hawq/requirements/system-requirements.html) and [Select HAWQ Host Machines](../../hawq/install/select-hosts/index.html).

## Procedure <a id="section_kwy_f3j_5r"></a>
The Ambari plug-in for HAWQ is no longer a separate download, but now included in the HDB software installation package.

1.  Follow the instructions in [Setting up HDB Repositories](setup-hdb-repos/index.html) to set up local `yum` HDB repositories on the single system (call it `repo-node`) you choose to host the HDB software. This system must be accessible to all nodes in your HAWQ cluster. This system may be your Ambari server host if you choose.

2.  Log in to the Ambari server host machine as the `root` user.

7.  Install the HAWQ Ambari plug-in; the plug-in will be installed from the HDB repository on `repo-node` that you set up in the previous step:

    ``` shell
    $ yum install -y hawq-ambari-plugin
    ```
    
    Installing the Ambari HAWQ plug-in creates the directory `/var/lib/hawq` and installs required scripts and template files there.

6.  Ensure that the Ambari server is running:

    ``` shell
    $ ambari-server status
    
    Using python  /usr/bin/python
    Ambari-server status
    Ambari Server running
    ```

    If Ambari server is not running, execute:
    
    ``` shell
    $ ambari-server start
    ```

7.  Execute the `add-hawq.py` script to add the HDB repository to the Ambari server. 

    Note that you must provide different script options depending on whether you set up your repositories on the Ambari server host or on a different host:
    -  If you set up repositories on the Ambari server host:
       
        ``` shell
        $ cd /var/lib/hawq
        $ ./add-hawq.py --user <admin-username> --password <admin-password> --stack HDP-2.5
        ```
    -  If you set up your repositories on a host other than the Ambari server host:
    
        ``` shell
        $ cd /var/lib/hawq
        $ ./add-hawq.py --user <admin-username> --password <admin-password> --stack HDP-2.5 --hawqrepo <hdb-2.2.0.0-url> --addonsrepo <hdb-add-ons-2.2.0.0-url>
        ```

    **Note:** Substitute the correct Ambari administrator username and password for your system. Also substitute the correct URL to the HAWQ and HAWQ add-ons repos for the specific minor version of HDB. For example:
    
    ``` shell
    $ ./add-hawq.py --user admin --password admin --stack HDP-2.5 --hawqrepo http://myserver.example.com/hdb-2.2.0.0 --addonsrepo http://myserver.example.com/hdb-add-ons-2.2.0.0
    ```
    
    Execute `add-hawq.py -h` to view all available options for the script. 

7.  Restart the Ambari server:

    ``` shell
    $ ambari-server restart
    ```
    
    **Note:** You must restart the Ambari server before proceeding. The above command restarts only the Ambari server, but leaves other Hadoop services running.

8.  Access the Ambari web console at `http://ambari.server.hostname:8080`, and log in as the `admin` user. \(The default password is also `admin`.\) Verify that the HAWQ service is now available.
11. Select **HDFS**, then click the **Configs** tab.
12. Customize the HDFS configuration as needed.  If you are using Ambari 2.4, note that some of these parameters may already be configured for you:
    1.  On the **Settings** tab, change the DataNode setting **DataNode max data transfer threads** \(dfs.datanode.max.transfer.threads parameter \) to *40960*.
    1.  Select the **Advanced** tab and expand **DataNode**. Ensure that the **DataNode directories permission** \(dfs.datanode.data.dir.perm parameter\) is set to *750*.
    1.  Expand the **General** tab and change the **Access time precision** \(dfs.namenode.accesstime.precision parameter\) to *0*.
    1.  Expand **Advanced hdfs-site**. Set the following properties to their indicated values.

        **Note:** If a property described below does not appear in the Ambari UI, select **Custom hdfs-site** and click **Add property...** to add the property definition and set it to the indicated value.

        |Property|Setting|
        |--------|-------|
        |**dfs.allow.truncate**|true|
        |**dfs.block.access.token.enable**|*false* for an unsecured HDFS cluster, or *true* for a secure cluster|
        |**dfs.block.local-path-access.user**|gpadmin|
        |**HDFS Short-circuit read** \(**dfs.client.read.shortcircuit**\)|true|
        |**dfs.client.socket-timeout**|300000000|
        |**dfs.client.use.legacy.blockreader.local**|false|
        |**dfs.datanode.handler.count**|60|
        |**dfs.datanode.socket.write.timeout**|7200000|
        |**dfs.namenode.handler.count**|600|
        |**dfs.support.append**|true|

        **Note:** HAWQ requires that you enable `dfs.allow.truncate`. The HAWQ service will fail to start if `dfs.allow.truncate` is not set to `true`.

    2.  Expand **Advanced core-site**, then set the following properties to their indicated values:

        **Note:** If a property described below does not appear in the Ambari UI, select **Custom core-site** and click **Add property...** to add the property definition and set it to the indicated value.

        |Property|Setting|
        |--------|-------|
        |**ipc.client.connection.maxidletime**|3600000|
        |**ipc.client.connect.timeout**|300000|
        |**ipc.server.listen.queue.size**|3300|

13. Click **Save** and enter a name for the configuration change (for example, *HAWQ prerequisites*). Click **Save** again, then **OK**.
13. If Ambari indicates that a service must be restarted, click **Restart** and allow the service to restart before you continue.
13. Select **Actions \> Add Service** on the home page.
14. Select both **HAWQ** and **PXF** from the list of services, then click **Next** to display the Assign Masters page.
15. Select the hosts that should run the HAWQ Master and HAWQ Standby Master, or accept the defaults. The HAWQ Master and HAWQ Standby Master must reside on separate hosts. Click **Next** to display the Assign Slaves and Clients page.
>

    **Note:** Only the **HAWQ Master** and **HAWQ Standby Master** entries are configurable on this page. NameNode, SNameNode, ZooKeeper and others may be displayed for reference, but they are not configurable when adding the HAWQ and PXF services.
>

    **Note:** The HAWQ Master component must not reside on the same host that is used for Hive Metastore if the Hive Metastore uses the new PostgreSQL database. This is because both services attempt to use port 5432. If it is required to co-locate these components on the same host, provision a PostgreSQL database beforehand on a port other than 5432 and choose the “Existing PostgreSQL Database” option for the Hive Metastore configuration. The same restriction applies to the admin host, because neither the HAWQ Master nor the Hive Metastore can run on the admin host where the Ambari Server is installed.

16. On the Assign Slaves and Clients page, choose the hosts that will run HAWQ segments and PXF, or accept the defaults. The Add Service Wizard automatically selects hosts for the HAWQ and PXF services based on available Hadoop services.
>

    **Note:** PXF must be installed on the HDFS NameNode, the Standby NameNode \(if configured\), *and* on each HDFS DataNode. A HAWQ segment must be installed on each HDFS DataNode.
>

    Click **Next** to continue.

17.  On the Customize Services page, the **Settings** tab configures basic properties of the HAWQ cluster. In most cases you can accept the default values provided on this page. Several configuration options may require attention depending on your deployment:
    *  **HAWQ Master Directory**, **HAWQ Segment Directory**: This specifies the base path for the HAWQ master or segment data directory.
    *  **HAWQ Master Temp Directories**, **HAWQ Segment Temp Directories**: HAWQ temporary directories are used for spill files. Enter one or more directories in which the HAWQ Master or a HAWQ segment stores these temporary files. Separate multiple directories with a comma. Any directories that you specify must already be available on all host machines. If you do not specify master or segment temporary directories, temporary files are stored in `/data/hawq/tmp/[master|segment]`.<br/><br/>As a best practice, use multiple master and segment temporary directories on separate, large disks (2TB or greater) to load balance writes to temporary files \(for example, `/disk1/tmp,/disk2/tmp`\). For a given query, HAWQ will use a separate temp directory (if available) for each virtual segment to store spill files. Multiple HAWQ sessions will also use separate temp directories where available to avoid disk contention. If you configure too few temp directories, or you place multiple temp directories on the same disk, you increase the risk of disk contention or running out of disk space when multiple virtual segments target the same disk. Each HAWQ segment node can have 6 virtual segments.
    *  **Resource Manager**: Select the resource manager to use for allocating resources in your HAWQ cluster. If you choose **Standalone**, HAWQ exclusively uses resources from the whole cluster. If you choose **YARN**, HAWQ contacts the YARN resource manager to negotiate resources. You can change the resource manager type after the initial installation. You will also have to configure some YARN-related properties in step 22. For more information on using YARN to manage resources, see [Managing Resources](../../hawq/resourcemgmt/HAWQResourceManagement/index.html).

		**Caution:** If you are installing HAWQ in secure mode (Kerberos-enabled), then set **Resource Manager** to **Standalone** to avoid encountering a known installation issue. You can enable YARN mode post-installation if YARN resource management is desired in HAWQ.  
    *  **VM Overcommit**: Set this value according to the instructions in the [System Requirements](../../hawq/requirements/system-requirements/index.html) document.

17.  Click the **Advanced** tab and enter a **HAWQ System User Password**. Retype the password where indicated.
>

	**Note:** Be sure to make appropriate user and password system administrative changes in order to prevent operational disruption. For example, you may need to disable the password expiration policy for this HAWQ System User account.
>
17. \(Optional.\) On the **Advanced** tab, you can change numerous configuration properties for the HAWQ cluster. Hover your mouse cursor over the entry field to display help for the associated property.  Default values are generally acceptable for a new installation. The following properties are sometimes customized during installation:

    |Property|Action|
    |--------|------|
    |**General > HAWQ DFS URL**|The URL that HAWQ uses to access HDFS|
    |**General > HAWQ Master Port**|Enter the port to use for the HAWQ master host or accept the default, 5432. **CAUTION:** If you are installing HAWQ in a single-node environment \(or when the Ambari server and HAWQ are installed the same node\) then *you cannot accept the default port*. Enter a unique port for the HAWQ master|
    |**General > HAWQ Segment Port**|The base port to use for HAWQ segment hosts|
    
    **Note:** Verify that all port numbers you select are unused and are available for use on your HAWQ machines.

1.  If you selected YARN as the **Resource Manager**, then you must configure several YARN properties for HAWQ. On the **Advanced** tab of HAWQ configuration, enter values for the following parameters:

	|Property|Action|
    |--------|------|
    |**Advanced hawq-site > hawq\_rm\_yarn\_address**|Specify the address and port number of the YARN resource manager server \(the value of `yarn.resourcemanager.address`\). For example: rm1.example.com:8050|
    |**Advanced hawq-site > hawq\_rm\_yarn\_queue\_name**|Specify the YARN queue name to use for registering the HAWQ resource manager. For example: `default` **Note:** If you specify a custom queue name other than `default`, you must configure the YARN scheduler and custom queue capacity, either through Ambari or directly in YARN's configuration files. See [Integrating YARN with HAWQ](../../hawq/resourcemgmt/YARNIntegration/index.html) for more details. |
    |**Advanced hawq-site > hawq\_rm\_yarn\_scheduler\_address**|Specify the address and port number of the YARN scheduler server \(the value of `yarn.resourcemanager.scheduler.address`\). For example: rm1.example.com:8030|

	If you have enabled high availability for YARN, then verify that the following values have been populated correctly in HAWQ:

	|Property|Action|
    |--------|------|
	|**Custom yarn-client > yarn.resourcemanager.ha**|Comma-delimited list of the fully qualified hostnames of the resource managers. When high availability is enabled, YARN ignores the value in hawq\_rm\_yarn\_address and uses this property’s value instead. For example, `rm1.example.com:8050,rm2.example.com:8050` |
	|**Custom yarn-client > yarn.resourcemanager.scheduler.ha**|Comma-delimited list of fully qualified hostnames of the resource manager schedulers. When high availability is enabled, YARN ignores the value in hawq\_rm\_yarn\_scheduler\_address and uses this property’s value instead. For example,`rm1.example.com:8030,rm2.example.com:8030` |

18.  Click **Next** to continue the installation. (Depending on your cluster configuration, Ambari may recommend that you change other properties before proceeding.)
18. Review your configuration choices, then click **Deploy** to begin the installation. Ambari now begins to install, start, and test the HAWQ and PXF configuration. During this procedure, you can click on the **Message** links to view the console output of individual tasks.

18.  Click **Next** after all tasks have completed. Review the summary of the install process, then click **Complete**.  Ambari may indicate that components on cluster need to be restarted. Choose **Restart > Restart All Affected** if necessary.
 
8.  (Optional) If you enabled temporary password-based authentication while preparing/configuring your HAWQ host systems, turn off password-based authentication as described in [Apache HAWQ System Requirements](../../hawq/requirements/system-requirements.html#topic_pwdlessssh).

19. To verify that HAWQ is installed, login to the HAWQ master as `gpadmin` and perform some basic commands:

    1.  Source the `greenplum_path.sh` file to set your environment for HAWQ:

        ``` shell
        $ source /usr/local/hawq/greenplum_path.sh
        ```

    1.  If you use a custom HAWQ master port number, set it in your environment. For example:

        ``` shell
        $ export PGPORT=5432
        ```
    
        Setting `PGPORT` simplifies `psql` invocation by providing a default for the `-p` (port) option. Add this setting to your `.bash_profile` to set the environment variable automatically each time you log in.

    1.  If you will routinely operate on a specific database, make this database the default by setting the `PGDATABASE` environment variable:

        ``` shell
        $ export PGDATABASE=databasename
        ```
    
        Setting `PGDATABASE` simplifies `psql` invocation by providing a default for the `-d` (database) option. Also add this setting to your `.bash_profile` to set the environment variable automatically each time you log in.
        
    1.  Start the `psql` interactive utility, connecting to the postgres database:

        ``` shell
        $ psql -d postgres
        ```
        ``` pre
        psql (8.2.15)
        Type "help" for help.
        postgres=#
        ```

    3.  Create a new database and connect to it:

        ``` sql
        postgres=# CREATE DATABASE mytest;
        ```
        ``` pre
        CREATE DATABASE
        postgres=# \c mytest
        You are now connected to database "mytest" as user "*username*".
        ```

    4.  Create a new table and insert sample data:

        ``` sql
        mytest=# CREATE TABLE t (i int);
        CREATE TABLE
        mytest=# INSERT INTO t SELECT generate_series(1,100);
        ```

    5.  Activate timing and perform a simple query:

        ``` pre
        mytest=# \timing
        Timing is on.
        ```
        
        ``` sql
        mytest=# SELECT count(*) FROM t;
        ```

        ``` pre
        count
        -------
        100
        (1 row)
        Time: 7.266 ms
        ```

## Post-Install Procedure for HDB 2.2.0 (Required)<a id="post-install-212-req"></a>

All HDB 2.2.0 deployments that use PXF must perform this procedure after installing/upgrading PXF to version 3.2.x. If you have not done so already, you must manually modify the `Hive*` profile definitions in the Ambari-managed `pxf-profiles.xml` configuration file to conform to new profile definitions.

Perform the following steps to update your Hive profile definitions:

1. Log in to the Ambari console.

2. Click on the `PXF` service in the left pane and select the `Configs` tab.

3. Expand the `Advanced pxf-profiles` field.

4. Scroll through the `pxf-profiles` text and locate the `Hive`, `HiveRC`, and `HiveText` profile definitions.

5. The **bold** text below identifies potential differences between the HDB 2.2.0 `Hive*` profile definitions and that of previous versions. You must add the appropriate `outputFormat` property to each Hive profile. Ensure your Hive profiles are configured as follows:

    <pre>
    &lt;profile&gt;
        &lt;name&gtHive&lt;/name&gt;
        &lt;description&gt;
            ...
        &lt;/description&gt;
        &lt;plugins&gt;
            &lt;fragmenter&gt;org.apache.hawq.pxf.plugins.hive.HiveDataFragmenter&lt;/fragmenter&gt;
            &lt;accessor&gt;org.apache.hawq.pxf.plugins.hive.HiveAccessor&lt;/accessor&gt;
            &lt;resolver&gt;org.apache.hawq.pxf.plugins.hive.HiveResolver&lt;/resolver&gt;
            &lt;metadata&gt;org.apache.hawq.pxf.plugins.hive.HiveMetadataFetcher&lt;/metadata&gt;
            <b>&lt;outputFormat&gtorg.apache.hawq.pxf.service.io.GPDBWritable&lt;/outputFormat&gt;</b>
        &lt;/plugins&gt;
    &lt;/profile&gt;
    &lt;profile&gt;
        &lt;name&gt;HiveRC&lt;/name&gt;
        &lt;description&gt;
            ...
        &lt;/description&gt;
        &lt;plugins&gt;
            &lt;fragmenter&gt;org.apache.hawq.pxf.plugins.hive.HiveInputFormatFragmenter&lt;/fragmenter&gt;
            &lt;accessor&gt;org.apache.hawq.pxf.plugins.hive.HiveRCFileAccessor&lt;/accessor&gt;
            &lt;resolver&gt;org.apache.hawq.pxf.plugins.hive.HiveColumnarSerdeResolver&lt;/resolver&gt;
            &lt;metadata&gt;org.apache.hawq.pxf.plugins.hive.HiveMetadataFetcher&lt;/metadata&gt;
            <b>&lt;outputFormat&gt;org.apache.hawq.pxf.service.io.Text&lt;/outputFormat&gt;</b>
        &lt;/plugins&gt;
    &lt;/profile&gt;
    &lt;profile&gt;
        &lt;name&gt;HiveText&lt;/name&gt;
        &lt;description&gt;
            ...
        &lt;/description&gt;
        &lt;plugins&gt;
            &lt;fragmenter&gt;org.apache.hawq.pxf.plugins.hive.HiveInputFormatFragmenter&lt;/fragmenter&gt;
            &lt;accessor&gt;org.apache.hawq.pxf.plugins.hive.HiveLineBreakAccessor&lt;/accessor&gt;
            &lt;resolver&gt;org.apache.hawq.pxf.plugins.hive.HiveStringPassResolver&lt;/resolver&gt;
            &lt;metadata&gt;org.apache.hawq.pxf.plugins.hive.HiveMetadataFetcher&lt;/metadata&gt;
            <b>&lt;outputFormat&gt;org.apache.hawq.pxf.service.io.Text&lt;/outputFormat&gt;</b>
        &lt;/plugins&gt;
    &lt;/profile&gt;
    </pre>

6. If you have enabled ORC file format support, locate the `HiveORC` profile definition and apply similar updates. If you have not yet enabled ORC file format support and intend to, copy/paste the complete `HiveORC` profile definition into the `Advanced pxf-profiles` pane:

    <pre>
    &lt;profile&gt;
        &lt;name&gt;HiveORC&lt;/name&gt;
        &lt;description&gt;
            ...
        &lt;/description&gt;
        &lt;plugins&gt;
            &lt;fragmenter&gt;org.apache.hawq.pxf.plugins.hive.HiveInputFormatFragmenter&lt;/fragmenter&gt;
            &lt;accessor&gt;org.apache.hawq.pxf.plugins.hive.HiveORCAccessor&lt;/accessor&gt;
            &lt;resolver&gt;org.apache.hawq.pxf.plugins.hive.HiveORCSerdeResolver&lt;/resolver&gt;
            &lt;metadata&gt;org.apache.hawq.pxf.plugins.hive.HiveMetadataFetcher&lt;/metadata&gt;
            <b>&lt;outputFormat&gt;org.apache.hawq.pxf.service.io.GPDBWritable&lt;/outputFormat&gt;</b>
        &lt;/plugins&gt;
    &lt;/profile&gt;
    </pre>

7. Press the `Save` button, and add a note to the `Save Configuration` dialog if you choose. `Save` again and `OK` the `Service Configuration Changes` dialog.

8. You must restart the PXF service after adding a new profile. Click the now orange-colored`Restart` button to `Restart All Affected`.

9. After the PXF service restarts, the Hive plug-in and associated profiles will be configured appropriately for use in your Ambari-managed HDB 2.2.0 cluster.


## Post-Install Procedure for Hive and HBase<a id="post-install-pxf"></a>

If you plan to access Hive or HBase with PXF, perform the post-install procedures identified in [PXF Post-Installation Procedures for Hive/HBase](postinstall-pxf-hbasehive/index.html) to complete the installation and configuration of the associated PXF plug-ins.

## Post-Install Procedure for JSON<a id="post-install-json"></a>

If you upgraded from Ambari 2.2.2 and plan to use the PXF JSON plug-in, you must explicitly add the JSON profile definition to the PXF service configuration:

1. Click on the `PXF` service in the left pane and select the `Configs` tab.

2. Expand the `Advanced pxf-profiles` field.

3. Scroll to the bottom of the `pxf-profiles` text block and copy/paste the following definition just above the `</profiles>` line (notice the `s`).

    ``` xml
    <profile>
        <name>Json</name>
        <description>
            ...
        </description>
        <plugins>
            <fragmenter>org.apache.hawq.pxf.plugins.hdfs.HdfsDataFragmenter</fragmenter>
            <accessor>org.apache.hawq.pxf.plugins.json.JsonAccessor</accessor>
            <resolver>org.apache.hawq.pxf.plugins.json.JsonResolver</resolver>
        </plugins>
    </profile>
    ```

4. Press the `Save` button, and add a note to the `Save Configuration` dialog if you choose.  `Save` again and `OK` the `Service Configuration Changes` dialog.

5. You must restart the PXF service after adding a new profile. Select the now orange colored`Restart` button to `Restart All Affected`.

6. When PXF restart is complete, the JSON plug-in will be available for use in your Ambari-managed HDB 2.2.0 cluster.

---
title: PXF Post-Installation Procedures for Hive/HBase
---

## <a id="post-install-pxf-hbasehive"></a>Post-Install Procedures for Hive and HBase

Perform the following post-install procedures if you plan to use PXF to access HBase and Hive. Differing procedures may be required for Ambari- vs. command-line-managed clusters.

1. Install the Hive client (`hive-client`) and HBase client (`hbase-client`) on each machine where you intend to install PXF Agents.

    **Command Line**: Install the Hive and/or HBase clients per the instructions in the HDP documentation. 
    
    **Ambari**: Install all clients on each PXF node during initial cluster configuration.  Alternatively, you can selectively install specific clients on a single host after cluster configuration and deployment by:
       - Selecting `Hosts` from the top menubar.  
       - Clicking on a specific host name.  
       - Clicking the `+Add` button in the upper right corner of the `Components` box.
       - Selecting `Hive Client` or `HBase Client` from the list of options.
   
      Make sure to install the desired clients on each PXF node in your cluster.

2. Update the HBase class path.

    **Note**: Perform these steps if you manage your HAWQ cluster from the command line *or* if you manage your cluster with Ambari and you *upgraded from Ambari 2.2.2*.

   1. Use either a text editor (command-line-managed cluster) or the Ambari Web interface to add the following line to the `hbase-env.sh` file.  

        ``` bash
        export HBASE_CLASSPATH=${HBASE_CLASSPATH}:/usr/lib/pxf/pxf-hbase.jar
        ```

   2. Restart HBase.  


### <a id="pxf-hivehbase-kerberos"></a>Kerberos Configuration (Optional)

If you are using Kerberos to secure Hive and HBase, you must configure proxy users, enable user impersonation, and configure PXF access to tables.

1. For secure Hive installations, use a text editor (command-line-managed cluster) or the Ambari Web interface to add the following property to the `hive-site.xml` file:

    ``` xml
    <property>
      <name>hive.server2.enable.impersonation</name>
      <description>Enable user impersonation for HiveServer2</description>
      <value>true</value>
    </property>
    ```

3. For secure Hive and HBase installations, use a text editor (command-line-managed cluster) or the Ambari Web interface to add the following properties to the `core-site.xml` file:

    ``` xml
    <property>
      <name>hadoop.proxyuser.hive.hosts</name>
      <value>*</value>
    </property>

    <property>
      <name>hadoop.proxyuser.hive.groups</name>
      <value>*</value>
    </property>

    <property>
      <name>hadoop.proxyuser.hbase.hosts</name>
      <value>*</value>
    </property>

    <property>
      <name>hadoop.proxyuser.hbase.groups</name>
      <value>*</value>
    </property>
    ```

4.  Restart both Hive and HBase. This is required for the new property values to take effect.

5. To use PXF with HBase or Hive tables, you must grant the `pxf` user read permission on those tables.

    **HBase**: Use the `GRANT` command for each table that you want to access with PXF. For example:

    ```
    hbase(main):001:0> grant 'pxf', 'R', 'my_table'
    ```

    **Hive**: Because Hive uses HDFS ACLs for access control, ensure that the `pxf` user has read permission on all of the HDFS directories that map to your database, tables, and partitions.

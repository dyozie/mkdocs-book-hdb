---
title: Install HAWQ from the Command Line (Optional)
---

This section provides instructions for installing a HAWQ system.

**Note:** Install HAWQ from the command line only if you do not use Ambari to install and manage HDFS. If you use Ambari for HDFS and management, follow the instructions in [Install HAWQ using Ambari](install-ambari/index.html) instead.

## Prerequisites <a id="cliprereq"></a>

Install a compatible HDFS cluster before you attempt to install HAWQ.

Select and configure all host machines that will run the HAWQ and PXF services. See [Apache HAWQ System Requirements](../../hawq/requirements/system-requirements.html) and [Select HAWQ Host Machines](../../hawq/install/select-hosts/index.html).

## Prepare Host Machines <a id="topic_eqn_fc4_15"></a>

Configure operating system parameters on each host machine before you begin to install HAWQ.

1.  Use a text editor to edit the `/etc/sysctl.conf` file. Add or edit each of the following parameter definitions to set the required value:

    ```
    kernel.shmmax = 1000000000
    kernel.shmmni = 4096
    kernel.shmall = 4000000000
    kernel.sem = 250 512000 100 2048
    kernel.sysrq = 1
    kernel.core_uses_pid = 1
    kernel.msgmnb = 65536
    kernel.msgmax = 65536
    kernel.msgmni = 2048
    net.ipv4.tcp_syncookies = 0
    net.ipv4.conf.default.accept_source_route = 0
    net.ipv4.tcp_tw_recycle = 1
    net.ipv4.tcp_max_syn_backlog = 200000
    net.ipv4.conf.all.arp_filter = 1
    net.ipv4.ip_local_port_range = 1281 65535
    net.core.netdev_max_backlog = 200000
    vm.overcommit_memory = 2
    fs.nr_open = 3000000
    kernel.threads-max = 798720
    kernel.pid_max = 798720
    # increase network
    net.core.rmem_max=2097152
    net.core.wmem_max=2097152
    ```

    Save the file after making the required changes.

2.  Execute the following command to apply your updated `/etc/sysctl.conf` file to the operating system configuration:

    ``` shell
    $ sysctl -p
    ```

3.  Use a text editor to edit the `/etc/security/limits.conf` file. Add the following definitions in the exact order that they are listed:

    ``` pre
    * soft nofile 2900000
    * hard nofile 2900000
    * soft nproc 131072
    * hard nproc 131072
    ```

    Save the file after making the required changes.

4.  Use a text editor to edit the `hdfs-site.xml` file. Ensure that the following HDFS parameters are set to the correct value:

    |Property|Setting|
    |--------|-------|
    |**dfs.allow.truncate**|true|
    |**dfs.block.access.token.enable**|*false* for an unsecured HDFS cluster, or *true* for a secure cluster|
    |**dfs.block.local-path-access.user**|gpadmin|
    |**dfs.client.read.shortcircuit**|true|
    |**dfs.client.socket-timeout**|300000000|
    |**dfs.client.use.legacy.blockreader.local**|false|
    |**dfs.datanode.data.dir.perm**|750|
    |**dfs.datanode.handler.count**|60|
    |**dfs.datanode.max.transfer.threads**|40960|
    |**dfs.datanode.socket.write.timeout**|7200000|
    |**dfs.namenode.accesstime.precision**|0|
    |**dfs.namenode.handler.count**|600|
    |**dfs.support.append**|true|

4.  Use a text editor to edit the `core-site.xml` file. Ensure that the following HDFS parameters are set to the correct value:

    |Property|Setting|
    |--------|-------|
    |**ipc.client.connection.maxidletime**|3600000|
    |**ipc.client.connect.timeout**|300000|
    |**ipc.server.listen.queue.size**|3300|

5.  Restart HDFS to apply your configuration changes.

6.  Ensure that the `/etc/hosts` file on each cluster node contains the hostname of every other member of the  cluster. Consider creating a single, master `/etc/hosts` file and either copying it or referencing it on every host that will take part in the cluster.

## Add New HDB Software Repositories<a id="20to201up_setupyum"></a>

Follow the instructions in [Setting up HDB Repositories](setup-hdb-repos/index.html) to set up local `yum` HDB repositories on the single system (call it `repo-node`) you choose to host the HDB software. This system must be accessible to all nodes in your HAWQ cluster.

After repo set up, each HAWQ host will be configured to obtain the HDB software from the `repo-node` HDB repositories.

## Install the HAWQ Cluster on Multiple Machines <a id="topic_ld1_bh4_15"></a>

Follow this procedure to install the HAWQ cluster on multiple host machines or VMs.

**Note:** If you want to install a cluster on a single host machine or VM, follow the instructions in [Install the HAWQ Cluster on a Single Machine](#topic_uqh_wg4_15) instead.

1.  Login to the target master host machine as the `root` user. If you are logged in as a different user, switch to the `root` account:

    ``` shell
    $ su - root
    ```
  
4.  Install HAWQ:

    ``` shell
    $ yum install -y hawq
    ```

5.  Source the `greenplum_path.sh` file to set your environment for HAWQ. For RPM installations, enter:

    ``` shell
    $ source /usr/local/hawq/greenplum_path.sh
    ```

    If you downloaded the tarball, substitute the path to the extracted `greenplum_path.sh` file \(for example `/opt/hawq-<version>/greenplum_path.sh`\).

6.  Use a text editor to create a file \(for example, hostfile\) that lists the hostname of the master node, the standby master node, and each segment node used in the cluster. Specify one hostname per line, as in the following example that defines a cluster with a master node, standby master node, and three segments:

    ```
    mdw
    smdw
    sdw1
    sdw2
    sdw3
    ```

    Create a second text file \(for example, seg\_hosts\) that specifies only the HAWQ segment hosts in your cluster. For example:

    ```
    sdw1
    sdw2
    sdw3
    ```

    You will use the hostfile and seg\_hosts files in subsequent steps to perform common configuration tasks on multiple nodes of the cluster.

7.  `hawq ssh-exkeys` creates and exchanges SSH keys for the `gpadmin` account, enabling passwordless SSH access between systems in your HAWQ cluster.  Execute the following command to set up passwordless SSH on the nodes identified in your hostfile:
 
    ``` shell
    $ hawq ssh-exkeys -f hostfile
    ```

10. Create the `gpadmin` user account on all hosts in the cluster; ensure that you set an appropriate password. For example:

    ``` shell
    $ hawq ssh -f hostfile -e '/usr/sbin/useradd gpadmin'
    $ hawq ssh –f hostfile -e 'echo -e "changeme\changeme" | passwd gpadmin'
    ```
>

	**Note:** Be sure to make appropriate user and password system administrative changes in order to prevent operational disruption. For example, you may need to disable the password expiration policy for the `gpadmin` account.
>
9.  Install HDB on all cluster hosts:

    ``` shell
    $ hawq ssh -f hostfile -e "yum install -y hawq"
    ```

11. Switch to the `gpadmin` user, and source the `greenplum_path.sh` file once again:

    ``` shell
    $ su - gpadmin
    $ source /usr/local/hawq/greenplum_path.sh
    ```

12. Execute the following `hawq` command a second time to set up passwordless SSH for the `gpadmin` user:

    ``` shell
    $ hawq ssh-exkeys -f hostfile
    ```
    
8.  (Optional) If you enabled temporary password-based authentication while preparing/configuring your HAWQ host systems, turn off password-based authentication as described in [Apache HAWQ System Requirements](../../hawq/requirements/system-requirements.html#topic_pwdlessssh).

13. Execute this command to confirm that HAWQ was installed on all of the hosts:

    ``` shell
    $ hawq ssh -f hostfile -e "ls -l $GPHOME"
    ```

14. Create a directory to use for storing the master catalog data. This directory must have sufficient disk space to store data, and it must be owned by the `gpadmin` user. For example:

    ``` shell
    $ mkdir -p /data/master
    $ chown -R gpadmin /data/master
    ```

    If you are using a standby master node, create the same directory on the standby host. For example:

    ``` shell
    $ hawq ssh -h smdw -e 'mkdir /data/master'
    $ hawq ssh -h smdw -e 'chown -R gpadmin /data/master'
    ```

    Replace `smdw` with the name of the standby host.

15. Use the following `hawq` commands with the seg\_hosts file you created earlier to create the data directory on all of the HAWQ segment hosts:

    ``` shell
    $ hawq ssh -f seg_hosts -e 'mkdir /data/segment'
    $ hawq ssh -f seg_hosts -e 'chown gpadmin /data/segment'
    ```

16. HAWQ temporary directories are used for spill files. Create the HAWQ temporary directories on all HAWQ hosts in your cluster. As a best practice, use a directory from each mounted drive available on your machines to load balance writing for temporary files. These drives are typically the same drives that are used by your DataNode service. For example, if you have two drives mounted on `/data1` and `/data2`, you could use `/data1/tmp` and `/data2/tmp` for storing temporary files.  If you do not specify master or segment temporary directories, temporary files are stored in `/tmp`.

    The following example commands use two disks with the paths `/data1/tmp` and `/data2/tmp`:

    ``` shell
    $ dirs="/data1/tmp /data2/tmp"
    $ mkdir -p $dirs
    $ chown -R gpadmin $dirs
    $ hawq ssh -h smdw -e "mkdir -p $dirs"
    $ hawq ssh -h smdw -e "chown -R gpadmin $dirs"
    $ hawq ssh -f seg_hosts -e "mkdir -p $dirs"
    $ hawq ssh -f seg_hosts -e "chown -R gpadmin $dirs"
    ```

    If you configure too few temp directories, or you place multiple temp directories on the same disk, you increase the risk of disk contention or running out of disk space when multiple virtual segments target the same disk. Each HAWQ segment node can have 6 virtual segments.
17. Login to the master host as the `gpadmin` user. Create a customized `$GPHOME/etc/hawq-site.xml` file using the template `$GPHOME/etc/template-hawq-site.xml` file for a multi-node cluster. Your custom `hawq-site.xml` should include the following modifications:
    1.  Change the `hawq_dfs_url` property definition to use the actual Namenode port number as well as the HAWQ data directory:

        ``` xml
            <property>
                <name>hawq_dfs_url</name>
                <value>localhost:8020/hawq_default</value>
                <description>URL for accessing HDFS.</description>
            </property>

        ```

        If HDFS is configured with NameNode high availability (HA), `hawq_dfs_url` should instead include the service ID that you configured. For example, if you configured HA with the service name `hdpcluster` the entry would be similar to:

        ``` xml
            <property>
                <name>hawq_dfs_url</name>
                <value>hdpcluster/hawq_default</value>
                <description>URL for accessing HDFS.</description>
            </property>

        ```

        Also set `gpadmin` as the owner of the parent directory HDFS directory you specify. For example:

        ``` shell
        $ hdfs dfs -chown gpadmin /
        ```

    2.  **Note:** Verify that all port numbers you select are unused and are available for use on your HAWQ machines.  
        Configure additional properties to specify the host names, port numbers, and directories used in your system. For example:

        |Property|Example Value|
        |--------|-------------|
        |hawq\_master\_address\_host|mdw|
        |hawq\_master\_address\_port|5432|
        |hawq\_standby\_address\_host|smdw|
        |hawq\_segment\_address\_port|40000|
        |hawq\_master\_directory|/data/master|
        |hawq\_segment\_directory|/data/segment|
        |hawq\_master\_temp\_directory|/data1/tmp,/data2/tmp|
        |hawq\_segment\_temp\_directory|/data1/tmp,/data2/tmp|
        |hawq\_global\_rm\_type|none|

        **Caution:** If you are installing HAWQ in [secure mode (Kerberos-enabled)](#topic_iwy_5fb_1t), then set `hawq_global_rm_type` to standalone mode (`none`) to avoid encountering a known installation issue. You can enable YARN mode post-installation if YARN resource management is desired in HAWQ.  

	3. If you wish to use YARN mode for HAWQ resource management, configure YARN properties for HAWQ. For example, in `$GPHOME/etc/hawq-site.xml`:

	    |Property|Example Value|Comment|
        |--------|-------------|-------|
        |hawq\_global\_rm\_type|yarn| When set to `yarn`, HAWQ requires that you configure additional YARN configuration parameters (`hawq_rm_yarn_address` or `hawq_rm_yarn_scheduler_address` |
		|hawq\_rm\_yarn\_address|mdw:9980 | This property must match the `yarn.resourcemanager.address` value in `yarn-site.xml`.|
        |hawq\_rm\_yarn\_scheduler\_address|mdw:9981 | This property must match the `yarn.resourcemanager.scheduler.address` value in `yarn-site.xml`.|


        If you have high availability enabled for YARN resource managers, then you must also configure the HA parameters in `$GPHOME/etc/yarn-client.xml`. For example:


        |Property|Example Value|Comment|
        |--------|-------------|-------|
        |yarn.resourcemanager.ha| rm1.example.com:8032, rm2.example.com:8032| Comma-delimited list of resource manager hosts. When high availability is enabled, YARN ignores the value in hawq\_rm\_yarn\_address and uses this property's value instead.|
        |yarn.resourcemanager.scheduler.ha|rm1.example.com:8030, rm2.example.com:8030 | Comma-delimited list of scheduler hosts. When high availability is enabled, YARN ignores the value in hawq\_rm\_yarn\_scheduler\_address and uses this property's value instead.|

        Replace the example hostnames with the fully qualified domain names of your resource manager host machines.

18. Edit the `$GPHOME/etc/slaves` file to list all of the segment host names for your cluster. For example:

    ```
    sdw1
    sdw2
    sdw3
    ```

8.  If your HDFS cluster is configured with NameNode high availability (HA), edit the `${GPHOME}/etc/hdfs-client.xml` file on each segment and add the following NameNode properties:

    ``` xml
    <property>
     <name>dfs.ha.namenodes.hdpcluster</name>
     <value>nn1,nn2</value>
    </property>

    <property>
     <name>dfs.namenode.http-address.hdpcluster.nn1</name>
     <value>ip-address-1.mycompany.com:50070</value>
    </property>

    <property>
     <name>dfs.namenode.http-address.hdpcluster.nn2</name>
     <value>ip-address-2.mycompany.com:50070</value>
    </property>

    <property>
     <name>dfs.namenode.rpc-address.hdpcluster.nn1</name>
     <value>ip-address-1.mycompany.com:8020</value>
    </property>

    <property>
     <name>dfs.namenode.rpc-address.hdpcluster.nn2</name>
     <value>ip-address-2.mycompany.com:8020</value>
    </property>

    <property>
     <name>dfs.nameservices</name>
     <value>hdpcluster</value>
    </property>
    ```

    In the listing above:
    * Replace `hdpcluster` with the actual service ID that is configured in HDFS.
    * Replace `ip-address-2.mycompany.com:50070` with the actual NameNode RPC host and port number that is configured in HDFS.
    * Replace `ip-address-1.mycompany.com:8020` with the actual NameNode HTTP host and port number that is configured in HDFS.
    * The order of the NameNodes listed in `dfs.ha.namenodes.hdpcluster` is important for performance, especially when running secure HDFS. The first entry (`nn1` in the example above) should correspond to the active NameNode.

19. Synchronize the customized `hawq-site.xml` and slaves files to all cluster nodes:

    ``` shell
    $ hawq scp -f hostfile hawq-site.xml slaves =:$GPHOME/etc/
    ```

21. Finally, initialize and start the new HAWQ cluster using the command:

    ``` shell
    $ hawq init cluster
    ```

    After the cluster starts, you can follow the instructions in [Validate the Installation](#topic_opr_3gp_15).


## Install the HAWQ Cluster on a Single Machine <a id="topic_uqh_wg4_15"></a>

Follow this procedure to install HAWQ software on a single host machine.

1.  Login to the target machine as the `root` user. If you are logged in as a different user, switch to the `root` account:

    ``` shell
    $ su - root
    ```

10. Create the `gpadmin` user account; ensure that you set an appropriate password. For example:

    ``` shell
    $ /usr/sbin/useradd gpadmin
    $ echo -e "changeme\changeme" | passwd gpadmin
    ```
>

	**Note:** Be sure to make appropriate user and password system administrative changes in order to prevent operational disruption. For example, you may need to disable the password expiration policy for the `gpadmin` account.
>

4.  Install HAWQ:

    ``` shell
    $ yum install -y hawq
    ```

5.  Switch to the `gpadmin` user:

    ``` shell
    $ su - gpadmin
    ```

6.  Source the `greenplum_path.sh` file to set up your environment for HAWQ. For RPM installations, enter:

    ``` shell
    $ source /usr/local/hawq/greenplum_path.sh
    ```

    If you downloaded the tarball, substitute the path to the extracted `greenplum_path.sh` file \(for example `/opt/hawq-<version>/greenplum_path.sh`\).

7.   HAWQ requires passwordless SSH access to all cluster nodes, even on a single-node cluster installation. Execute the following `hawq` command to exchange keys and enable passwordless SSH to localhost:

    ``` shell
    $ hawq ssh-exkeys -h localhost
    ```
    
8.  (Optional) Turn off temporary password-based authentication as described in [Apache HAWQ System Requirements](../../hawq/requirements/system-requirements.html#topic_pwdlessssh).

8.  If your HDFS Namenode does not use the default port, 8020, then open the `$GPHOME/etc/hawq-site.xml` file with a text editor and modify the following property definition to use the actual Namenode port number:

    ``` xml
        <property>
            <name>hawq_dfs_url</name>
            <value>localhost:8020/hawq_default</value>
            <description>URL for accessing HDFS.</description>
        </property>

    ```

    Also ensure that the gpadmin user has both read and write access to the parent directory specified with `hawq_dfs_url`. For example:

    ``` shell
    $ hdfs dfs -chown gpadmin hdfs://localhost:8020/
    ```

9.  Finally, initialize and start the new HAWQ cluster using the command:

    ``` shell
    $ hawq init cluster
    ```

    After the cluster starts, you can follow the instructions in [Validate the Installation](#topic_opr_3gp_15).


## \(Optional\) Enable Kerberos <a id="topic_iwy_5fb_1t"></a>

The following additional steps are necessary only if you manage your system manually \(without using Ambari\), and you enabled Kerberos security for HDFS.

For manual installations, perform these additional steps after you complete the previous procedure:

1.  Ensure that the HDFS parameter `dfs.block.access.token.enable` is set to true for the secure HDFS cluster. This property can be set within Ambari via **Services \> HDFS \> Configs \> Advanced hdfs-site \> dfs.block.access.token.enable**. After modifying this parameter, you must restart HDFS.
2.  Login to the Kerberos Key Distribution Center \(KDC\) server as the `root` user.
3.  Use `kadmin.local` to create a new principal for the `postgres` user. This will be used by the HAWQ master segment host:

    ``` shell
    $ kadmin.local -q “addprinc -randkey postgres@LOCAL.DOMAIN”
    ```

4.  Use `kadmin.local` to generate a Kerberos service principal for all other hosts that will run a HAWQ segment with the PXF service. The service principal should be of the form `name/role@REALM` where:
    -   name is the PXF service user name \(`pxf`\). Use the same name for each HAWQ host.
    -   role is the DNS resolvable, fully-qualified hostname of the HAWQ host machine \(output of `hostname -f` command\).
    -   REALM is the Kerberos Distribution Center \(KDC\) realm used in the Hadoop cluster \(for example, `LOCAL.DOMAIN`\).

    You can generate all principals on the KDC server. For example, these commands add service principals for three HAWQ nodes on the hosts host1.example.com, host2.example.com, and host3.example.com:

    ``` shell
    $ kadmin.local -q "addprinc -randkey pxf/host1.example.com@LOCAL.DOMAIN"
    $ kadmin.local -q "addprinc -randkey pxf/host2.example.com@LOCAL.DOMAIN"
    $ kadmin.local -q "addprinc -randkey pxf/host3.example.com@LOCAL.DOMAIN"
    ```
    Repeat this step for each host that runs a PXF service. Substitute the fully-qualified hostname of each host machine and the realm name used in your configuration.
>

    **Note:** As an alternative, if you have a hosts file that lists the fully-qualified domain name of each cluster host \(one host per line\), then you can generate principals using the command:

    ``` shell
    $ for HOST in $\(cat hosts\) ; do sudo kadmin.local -q "addprinc -randkey pxf/$HOST@LOCAL.DOMAIN" ; done
    ```

5.  Generate a keytab file for each principal that you created \(for the `postgres` and for each `pxf` service principal\). You can store the keytab files in any convenient location \(this example uses the directory `/etc/security/keytabs`\). You will deploy the service principal keytab files to their respective HAWQ host machines in a later step:

    ``` shell
    $ kadmin.local -q “xst -k /etc/security/keytabs/hawq.service.keytab postgres@LOCAL.DOMAIN”
    $ kadmin.local -q “xst -k /etc/security/keytabs/pxf-host1.service.keytab pxf/host1.example.com@LOCAL.DOMAIN”
    $ kadmin.local -q “xst -k /etc/security/keytabs/pxf-host2.service.keytab pxf/host2.example.com@LOCAL.DOMAIN”
    $ kadmin.local -q “xst -k /etc/security/keytabs/pxf-host3.service.keytab pxf/host3.example.com@LOCAL.DOMAIN”
    $ kadmin.local -q “listprincs”
    ```

    Repeat the `xst` command as necessary to generate a keytab for each HAWQ service principal that you created in the previous step.

6.  The HAWQ master server also requires a hdfs.headless.keytab file for the HDFS principal to be available under `/etc/security/keytabs`. If this file does not already exist, generate it using the command:

    ``` shell
    $ kadmin.local -q “addprinc -randkey hdfs@LOCAL.DOMAIN”
    $ kadmin.local -q “xst -k /etc/security/keytabs/hdfs.headless.keytab hdfs@LOCAL.DOMAIN”
    ```

    **Note:** HAWQ initialization will fail if the above keytab file is not available.

7.  Copy the HAWQ service keytab file \(and the hdfs.headless.keytab file if you created one\) to the HAWQ master segment host:

    ``` shell
    $ scp /etc/security/keytabs/hawq.service.keytab hawq_master_fqdn:/etc/security/keytabs/hawq.service.keytab
    $ scp /etc/security/keytabs/hdfs.headless.keytab hawq_master_fqdn:/etc/security/keytabs/hdfs.headless.keytab
    ```

8.  Change the ownership and permissions on hawq.service.keytab \(and on hdfs.headless.keytab if you copied it\) as follows:

    ``` shell
    $ ssh hawq_master_fqdn chown gpadmin:gpadmin /etc/security/keytabs/hawq.service.keytab
    $ ssh hawq_master_fqdn chmod 400 /etc/security/keytabs/hawq.service.keytab
    $ ssh hawq_master_fqdn chown hdfs:hdfs /etc/security/keytabs/hdfs.headless.keytab
    $ ssh hawq_master_fqdn chmod 440 /etc/security/keytabs/hdfs.headless.keytab
    ```

9.  Copy the keytab file for each service principal to its respective HAWQ host:

    ``` shell
    $ scp /etc/security/keytabs/pxf-host1.service.keytab host1.example.com:/etc/security/keytabs/pxf.service.keytab
    $ scp /etc/security/keytabs/pxf-host2.service.keytab host2.example.com:/etc/security/keytabs/pxf.service.keytab
    $ scp /etc/security/keytabs/pxf-host3.service.keytab host3.example.com:/etc/security/keytabs/pxf.service.keytab
    ```

    **Note:** Repeat this step for all of the keytab files that you created. Record the full path of the keytab file on each host machine. During installation, you will need to provide the path to finish configuring Kerberos for HAWQ.

10. Change the ownership and permissions on the pxf.service.keytab files:

    ``` shell
    $ ssh host1.example.com chown pxf:pxf /etc/security/keytabs/pxf.service.keytab
    $ ssh host1.example.com chmod 400 /etc/security/keytabs/pxf.service.keytab
    $ ssh host2.example.com chown pxf:pxf /etc/security/keytabs/pxf.service.keytab
    $ ssh host2.example.com chmod 400 /etc/security/keytabs/pxf.service.keytab
    $ ssh host3.example.com chown pxf:pxf /etc/security/keytabs/pxf.service.keytab
    $ ssh host3.example.com chmod 400 /etc/security/keytabs/pxf.service.keytab
    ```

    **Note:** Repeat this step for all of the keytab files that you copied.

11. After you have created and copied all required keytab files to their respective hosts, continue to install the HAWQ and PXF services if necessary.

    If you already installed HAWQ and PXF and you enabled HDFS security *without* using Ambari, follow the steps in [\(Optional\) Enable Kerberos](#topic_iwy_5fb_1t) to finish enabling HAWQ and PXF security.

12. On each PXF node, edit the `/etc/gphd/pxf/conf/pxf-site.xml` to specify the local keytab file and security principal. Add or uncomment the properties:

    ``` xml
    <property>
        <name>pxf.service.kerberos.keytab</name>
        <value>/etc/security/phd/keytabs/pxf.service.keytab</value>
        <description>path to keytab file owned by pxf service
        with permissions 0400</description>
    </property>

    <property>
        <name>pxf.service.kerberos.principal</name>
        <value>pxf/_HOST@PHD.LOCAL</value>
        <description>Kerberos principal pxf service should use.
        _HOST is replaced automatically with hostnames
        FQDN</description>
    </property>
    ```

    Also uncomment the KDC section in `yarn-client.xml` if you want to configure YARN for security.

13. Perform the remaining steps on the HAWQ master node as the `gpadmin` user:
    1.  Login to the HAWQ database master server as the `gpadmin` user:

        ``` shell
        $ ssh hawq_master_fqdn
        ```

    2.  Run the following commands to set environment variables:

        ``` shell
        $ source /usr/local/hawq/greenplum_path.sh
        ```

    3.  Run the following commands to enable security and configure the keytab file:

        ``` shell
        $ hawq config -c enable_secure_filesystem -v ON
        $ hawq config -c krb_server_keyfile -v /etc/security/keytabs/hawq.service.keytab
        ```

    4.  Start the HAWQ service:

        ``` shell
        $ hawq start cluster -a
        ```

    5.  Obtain a kerberos ticket and change the ownership and permissions of the HAWQ HDFS data directory:

        ``` shell
        $ sudo -u hdfs kinit -kt /etc/security/keytabs/hdfs.headless.keytab hdfs
        $ sudo -u hdfs hdfs dfs -chown -R gpadmin:gpadmin /hawq_data
        ```

        Substitute the actual HDFS data directory name for your system.

    6.  On the HAWQ master node and on all segment server nodes, edit the /usr/local/hawq/etc/hdfs-client.xml file to enable kerberos security and assign the HDFS NameNode principal. Add or uncomment the following properties in each file:

        ``` xml
        <property>
          <name>hadoop.security.authentication</name>
          <value>kerberos</value>
        </property>

        ```

    7.  If you are using YARN for resource management, edit the yarn-client.xml file to enable kerberos security. Add or uncomment the following property in the yarn-client.xml file on each HAWQ node:

        ``` xml
        <property>
          <name>hadoop.security.authentication</name>
          <value>kerberos</value>
        </property>
        ```

    8.  Restart HAWQ as the `gpadmin` user:

        ``` shell
        $ hawq restart cluster -a -M fast
        ```


## Validate the Installation <a id="topic_opr_3gp_15"></a>

Perform these basic commands to ensure that the new  cluster is functional.

1.  Ensure that you have initialized and started the new cluster by using the `hawq init cluster` command, as described in the previous procedures.

1.  Source the `greenplum_path.sh` file to set your environment for HAWQ. For RPM installations, enter:

    ``` shell
    $ source /usr/local/hawq/greenplum_path.sh
    ```

    If you downloaded the tarball, substitute the path to the extracted `greenplum_path.sh` file \(for example `/opt/hawq-<version>/greenplum_path.sh`\).

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
    
2.  Start the `psql` interactive utility, connecting to the postgres database:

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
    You are now connected to database "mytest" as user "<username>".
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


## Install PXF Plug-ins <a id="topic_pnk_4cv_25"></a>

If you plan on accessing data in external systems such as HDFS or JSON files, Hive or HBase:

1.  Install PXF on the external system. We recommend installing PXF on all nodes in your cluster. See [Installing PXF Plug-ins](../../hawq/pxf/InstallPXFPlugins/index.html).

2.  Follow the instructions in [PXF Post-Installation Procedures for Hive/HBase ](postinstall-pxf-hbasehive/index.html) to complete the PXF configuration.


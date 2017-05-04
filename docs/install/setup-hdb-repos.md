---
title: Setting up HDB Software Repositories
---

You must set up two local `yum` repositories before installing  HDB. 

Perform the following steps on the single system you choose to host the HDB software repositories. If you use Ambari to manage your HAWQ cluster, you may choose to set the HDB repositories up on your Ambari server host. This system (call it `repo-node`) must be accessible to all nodes in your HAWQ cluster. Log in and perform the following commands as the `root` user.


2. (Re)start the `httpd` server: 

    ``` shell
    root@repo-node$ service httpd [re]start
    ```
     
3. Download the HDB 2.x install package from [Pivotal Network](https://network.pivotal.io/products/pivotal-hdb). Make note of the directory to which the file was downloaded.

    The name format of the downloaded file is `hdb-<version>.elN-<build>.tar.gz`. For example, `hdb-2.2.0.0.el7-3991.tar.gz`.

4. Create a staging directory into which you will extract the HDB install package. The staging directory and all the directories above it must be readable and executable by the system user that runs the `httpd` process \(typically `apache`\). Make the directory readable and executable by all users. For example:

    ``` shell
    root@repo-node$ mkdir /staging
    root@repo-node$ chmod a+rx /staging
    ```
    
    **Note:** Do not use the `/tmp` directory as a staging directory; files under `/tmp` can be removed at any time.

5. The HDB install file includes an archived `yum` repository. Unpack the HDB install file and run the `setup_repo.sh` script to add the HDB software distribution to the local `yum` package repository. For example:

    ``` shell
    root@repo-node$ cd /staging
    root@repo-node$ tar -zxvf path_to/hdb-<version>-<build>.tar.gz
    root@repo-node$ cd hdb-<version>
    root@repo-node$ ./setup_repo.sh
    ```
   
    `setup_repo.sh` creates an HDB repository named `hdb-<version>.repo` on the local host. The script also creates a symlink from the document root of the `httpd` server \(`/var/www/html`\) to the directory in which the HDB `.tar.gz` file was unpacked.

6. (*Optional*) If you plan to install PL/R in your HAWQ cluster, download the HDB 2.x Add-Ons package from [Pivotal Network](https://network.pivotal.io/products/pivotal-hdb). The name format of the downloaded file is `hdb-add-ons-<version>.elN-<build>.tar.gz`. Unpack the downloaded file and run `setup_repo.sh` as described for the HDB repository above. An HDB Add-Ons repository named `hdb-add-ons-<version>.repo` will be created on the local host.

7. If you are performing a fresh install of HDB with Ambari, and `repo-node` is not the Ambari server host, update the Ambari server host to use the HDB software repositories just configured on `repo-node`: 

    ``` shell
    root@repo-node$ scp /etc/yum.repos.d/hdb-<version>.repo root@<ambari_server_host>:/etc/yum.repos.d/hdb-<version>.repo
    root@repo-node$ scp /etc/yum.repos.d/hdb-add-ons-<version>.repo root@<ambari_server_host>:/etc/yum.repos.d/hdb-add-ons-<version>.repo
    ```

7. Install the `epel-release` package on all nodes in your HAWQ cluster. You must have superuser privileges on the HAWQ systems. For example:

    ``` shell
    root@repo-node$ export HAWQNODES="hawq-master hawq-seg1 hawq-seg2 pxf1"
    root@repo-node$ for host in $HAWQNODES; do ssh $host "yum install -y epel-release;"; done
    ```
    
    If you require additional RHEL packages (assumes you have an appropriate RHEL subscription), perform the following on each node in your HAWQ cluster to enable the appropriate repo(s), substituting your RHEL version number for `N`. For example:

    ``` shell
    root@repo-node$ for host in $HAWQNODES; do ssh $host "yum install -y yum-utils"; done
    root@repo-node$ for host in $HAWQNODES; do ssh $host "yum-config-manager --enable rhel-N-server-eus-rpms"; done
    root@repo-node$ for host in $HAWQNODES; do ssh $host "yum-config-manager --enable rhel-N-server-optional-rpms"; done
    root@repo-node$ for host in $HAWQNODES; do ssh $host "yum-config-manager --enable rhel-N-server-rpms"; done
    ```

8. *If you are performing a fresh install of HDB with Ambari, skip the rest of the steps in this section*.

8. If you are upgrading an existing HDB installation or performing a fresh install of HDB from the command line, then update all HAWQ nodes in the cluster to use the HDB software repositories that you just configured on `repo-node`. You must have superuser privileges on the HAWQ systems. For example:

    ``` shell
    root@repo-node$ export HAWQNODES="hawq-master hawq-seg1 hawq-seg2 pxf1"
    root@repo-node$ for host in $HAWQNODES; do scp /etc/yum.repos.d/hdb-<version>.repo root@$host:/etc/yum.repos.d/hdb-<version>.repo; done
    root@repo-node$ for host in $HAWQNODES; do scp /etc/yum.repos.d/hdb-add-ons-<version>.repo root@$host:/etc/yum.repos.d/hdb-add-ons-<version>.repo; done
    ```
   
    Substitute the correct host names for the systems in your HAWQ cluster.

---
layout: layout.pug
navigationTitle:  Frequently Asked Questions
title: Frequently Asked Questions
menuWeight: 20
excerpt: Frequently asked questions about installing DC/OS
---


## Q. Can I install DC/OS on an already running Mesos cluster?
We recommend starting with a fresh cluster to ensure all defaults are set to expected values. This prevents unexpected conditions related to mismatched versions and configurations.

## Q. What are the OS requirements of DC/OS?
See the [system requirements](/mesosphere/dcos/1.12/installing/production/system-requirements/) documentation.

## Q. Does DC/OS install ZooKeeper?
DC/OS runs its own ZooKeeper supervised by Exhibitor and `systemd`.

## Q. Is it necessary to maintain a bootstrap node after the cluster is created?
If you specify an Exhibitor storage backend type other than `exhibitor_storage_backend: static` in your cluster configuration [file](/mesosphere/dcos/1.12/installing/production/advanced-configuration/configuration-reference/), you must maintain the external storage for the lifetime of your cluster to facilitate leader elections. If your cluster is mission critical, you should harden your external storage by using S3 or running the bootstrap ZooKeeper as a quorum. Interruptions of service from the external storage can be tolerated, but permanent loss of state can lead to unexpected conditions.

## Q. How can I add Mesos attributes to nodes to use Marathon constraints?

In DC/OS, add the line `MESOS_ATTRIBUTES=<key>:<value>` to the file `/var/lib/dcos/mesos-slave-common` (it may need to be created) for each attribute you want to add. See the [Mesos documentation](http://mesos.apache.org/documentation/latest/attributes-resources/) for more information.

## Q. How do I gracefully shut down an agent?

- To gracefully kill an agent node's Mesos process and allow `systemd` to restart it, run the following command.

    ```bash
    sudo systemctl kill -s SIGUSR1 dcos-mesos-slave
    ```

If Auto Scaling groups are in use, the node will be replaced automatically.

- For a public agent, run the following command:

    ```bash
    sudo systemctl kill -s SIGUSR1 dcos-mesos-slave-public
    ```

- To gracefully kill the process and prevent systemd from restarting it, add a `stop` command:

    ```bash
    sudo systemctl kill -s SIGUSR1 dcos-mesos-slave && sudo systemctl stop dcos-mesos-slave
    ```

- For a public agent, run the following command:

    ```bash
    sudo systemctl kill -s SIGUSR1 dcos-mesos-slave-public && sudo systemctl stop dcos-mesos-slave-public
    ```

<a name="iam-backup"></a>

## Q. How do I backup the IAM database?

- To backup the IAM database to a file run the following command on one of the master nodes:

```bash
dcos-shell iam-database-backup > ~/iam-backup.sql
```

## Q. How do I restore the IAM database?
You can restore the DC/OS identity and access management (IAM) database from a previously-created database backup file. Keep in mind that you cannot use a database backup file and the restore command-line utility to create a new database in a new DC/OS cluster. You should only use the database backup to restore the identity and access management database in the same cluster you used to create the backup file and in the same ZooKeeper state as the cluster was when you created the backup file.

- To restore the IAM database for a cluster from the backup file `~/iam-backup.sql`, run the following command on one of the master nodes:

```bash
dcos-shell iam-database-restore ~/iam-backup.sql
```

If the command is successful, the IAM database is restored from the backup file and the cluster returns to healthy operation.

<a name="zk-backup"></a>

## Q. How do I backup ZooKeeper using Guano?

There may be instances where you need to backup the state of ZooKeeper. Use the following instructions on agent or master node to backup ZooKeeper using Guano. 

<p class="message--important"><strong>IMPORTANT: </strong>The following instructions will not work on bootstrap node.</p>

1. Download the Guano ZooKeeper utility.

```bash
sudo wget https://s3.eu-central-1.amazonaws.com/adyatlov-public/guano-0.1a.jar.zip
```

2. Unzip the utility.

```bash
unzip guano-0.1a.jar.zip
```

3. Set the variable `ZKHOST` to a ZooKeeper endpoint.

```bash
export ZKHOST="zk-1.zk"
```

4. Run the following command to backup your ZooKeeper state.

<p class="message--note"><strong>NOTE: </strong>The user must enter the login credentials (username and password) that has permission to read the ZK store.</p>

```bash
/opt/mesosphere/bin/dcos-shell java -jar guano-0.1a.jar -u super -p secret -d / -o /tmp/mesos-zk-backup -s $ZKHOST:2181 && tar -zcvf zkstate.tar.gz /tmp/mesos-zk-backup/
```


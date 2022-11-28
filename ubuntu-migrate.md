---

copyright:
  years: 2022, 2022
lastupdated: "2022-11-28"

keywords: ubuntu, operating system, migrate, ubuntu version, worker nodes

subcollection: openshift

---

{{site.data.keyword.attribute-definition-list}}


# Migrating to a new Ubuntu version
{: #ubuntu-migrate}

Support for Ubuntu 20 is available on 6 December 2022. This information is available as a preview for the steps that are required to migrate your worker nodes after that date. 
{: note}



To migrate your worker nodes to a new Ubuntu version, you must provision a new worker pool. Then, add worker nodes to the new pool and remove the original worker pool. 
{: shortdesc}

For more information about creating worker pools and adding worker nodes, see [Adding worker nodes in classic clusters](/docs/containers?topic=containers-add_workers) or [Adding worker nodes in VPC clusters](/docs/containers?topic=containers-add_workers#vpc_pools).

### Migration steps
{: #ubuntu-migrate-steps}

Migrate your worker nodes to use Ubuntu 20. These steps apply to all supported cluster versions.
{: shortdesc}

1. In your cluster, create a new worker pool for the Ubuntu 20 worker nodes. Include the `--operating-system` option and specify `UBUNTU_20_64`. Make sure that the number of nodes specified with the `--size-per-zone` option matches the number of Ubuntu 20 worker nodes that are to be replaced.

    **For classic clusters**. See the [CLI reference](/docs/containers?topic=containers-kubernetes-service-cli#cs_worker_pool_create) for command details.

    ```sh
    ibmcloud ks worker-pool create classic --name <worker_pool_name> --cluster <cluster_name_or_ID> --flavor <flavor> --operating-system UBUNTU_20_64 --size-per-zone <number_of_workers_per_zone> 
    ```
    {: pre}

    **For VPC clusters**. See the [CLI reference](/docs/containers?topic=containers-kubernetes-service-cli#cli_worker_pool_create_vpc_gen2) for command details.

    ```sh
    ibmcloud ks worker-pool create vpc-gen2 --name <worker_pool_name> --cluster <cluster_name_or_ID> --flavor <flavor> --operating-system UBUNTU_20_64 --size-per-zone <number_of_workers_per_zone> 
    ```
    {: pre}

3. Verify that the worker pool is created.

    ```sh
    ibmcloud ks worker-pool ls --cluster <cluster_name_or_ID>
    ```
    {: pre}

4. Add a zone to your worker pool. When you add a zone, the number of worker nodes you specified with the `--size-per-zone` option are added to the zone. These worker nodes run the Ubuntu 20 operating system. 
    * [Adding a zone to a worker pool in a classic cluster](/docs/containers?topic=containers-add_workers#add_zone)
    * [Adding a zone to a worker pool in a VPC cluster](/docs/containers?topic=containers-add_workers#vpc_add_zone)

5. Verify that worker nodes are available in your new worker pool. In the output, find the listing for the new worker pool and check the number in the **Workers** column.
    ```sh
    ibmcloud ks worker-pool ls --cluster <cluster_name_or-ID>
    ```
    {: pre}

6. [Remove the worker pool](/docs/containers?topic=containers-kubernetes-service-cli#cs_worker_pool_rm) that contains the Ubuntu 18 worker nodes. 

    Consider scaling down your Ubuntu 18 worker pool and keeping it for several days before you remove it. This way, you can scale the worker pool back up if your workload experiences disruptions during the migration process. When you have determined that your workload is stable and functions normally, you can remove the Ubuntu 18 worker pool.
    {: important}

    1. List your worker pools and note the name of the worker pool you want to remove.
        ```sh
        ibmcloud ks worker-pool ls --cluster <cluster_name>
        ```
        {: pre}

    2. Run the command to remove the worker pool.
        ```sh
        ibmcloud ks worker-pool rm --worker-pool <worker_pool_name> --cluster <cluster_name>
        ```
        {: pre}



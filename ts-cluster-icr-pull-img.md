---

copyright: 
  years: 2014, 2021
lastupdated: "2021-09-24"

keywords: kubernetes, iks

subcollection: containers
content-type: troubleshoot

---



{{site.data.keyword.attribute-definition-list}}

# Why can't the cluster pull images from {{site.data.keyword.registrylong_notm}} during creation?
{: #ts_image_pull_create}

**Infrastructure provider**:
    * <img src="../images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
    * <img src="../images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC


When you created a cluster, you received an error message similar to the following.
{: tsSymptoms}


```
Your cluster cannot pull images from the {{site.data.keyword.registrylong_notm}} 'icr.io' domains because an IAM access policy could not be created. Make sure that you have the IAM Administrator platform access role to {{site.data.keyword.registrylong_notm}}. Then, create an image pull secret with IAM credentials to the registry by running 'ibmcloud ks cluster pull-secret apply'.
```
{: screen}


During cluster creation, a service ID is created for your cluster and assigned the **Reader** service access policy to {{site.data.keyword.registrylong_notm}}.
{: tsCauses}

Then, an API key for this service ID is generated and stored in [an image pull secret](/docs/containers?topic=containers-registry#cluster_registry_auth) to authorize the cluster to pull images from {{site.data.keyword.registrylong_notm}}.

To successfully assign the **Reader** service access policy to the service ID during cluster creation, you must have the **Administrator** platform access policy to {{site.data.keyword.registrylong_notm}}.
{: tsResolve}

Steps:
1. Make sure that the account owner gives you the **Administrator** role to {{site.data.keyword.registrylong_notm}}.
    ```
    ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
    ```
    {: pre}

2. [Use the `ibmcloud ks cluster pull-secret apply` command](/docs/containers?topic=containers-kubernetes-service-cli#cs_cluster_pull_secret_apply) to re-create an image pull secret with the appropriate registry credentials.






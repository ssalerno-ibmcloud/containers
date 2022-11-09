---

copyright: 
  years: 2022, 2022
lastupdated: "2022-10-26"

keywords: kubernetes, keepalive, TCP

subcollection: containers

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}



# Why does my pod with long running TCP connections get disconnected?
{: #ts-keepalive-sysctl}
{: support}

Supported infrastructure providers
:   Classic
:   VPC

When a pod has long running TCP connections and the pod is idle for a period of time, it occasionally gets disconnected.
{: tsSymptoms}

The idle time might have exceeded the limit that is defined by the `sysctl` keepalive settings for the pod.
{: tsCauses}

Try updating the the `sysctl` keepalive settings for the pod. For more information, see [Optimizing network keepalive `sysctl` settings](/docs/containers?topic=containers-kernel#keepalive-iks).
{: tsResolve}

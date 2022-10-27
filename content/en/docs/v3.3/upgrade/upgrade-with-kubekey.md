---
title: "Upgrade with KubeKey"
keywords: "Kubernetes, upgrade, KubeSphere, 3.3, KubeKey"
description: "Use KubeKey to upgrade Kubernetes and KubeSphere."
linkTitle: "Upgrade with KubeKey"
weight: 7200
---
KubeKey is recommended for users whose KubeSphere and Kubernetes were both installed by [KubeKey](../../installing-on-linux/introduction/kubekey/). If your Kubernetes cluster was provisioned by yourself or cloud providers, refer to [Upgrade with ks-installer](../upgrade-with-ks-installer/).

This tutorial demonstrates how to upgrade your cluster using KubeKey.

## Prerequisites

- You need to have a KubeSphere cluster running v3.2.x. If your KubeSphere version is v3.1.x or earlier, upgrade to v3.2.x first.
- Read [Release Notes for 3.3](../../../v3.3/release/release-v330/) carefully.
- Back up any important component beforehand.
- Make your upgrade plan. Two scenarios are provided in this document for [all-in-one clusters](#all-in-one-cluster) and [multi-node clusters](#multi-node-cluster) respectively.

## Major Updates

   Compared with previous versions, KubeSphere 3.3.1 has witnessed major updates of permission control. Platform-level built-in roles `users-manager` and `workspace-manager` are deleted and `platform-self-provisioner` is added. For more information about built-in roles, refer to [Create a user](../../quick-start/create-workspace-and-project/#Create a user). Therefore, before you upgrade KubeSphere to 3.3.1, please note the following:

   - Change of built-in roles: If a tenant has been bound to `users-manager` or `workspace-manager`, its role will be changed to `platform-regular`.
   - Change of user-defiend roles: As KubeSphere 3.3.1 has blocked some permissions of user-defiend roles. After you upgrade KubeSphere to 3.3.1, blocked permissions of user-defiend roles will be revoked, but user-defiend roles will be retained. Blocked permissions of users at different levels vary:
       - Platform-level roles: user management, role management, and workspace management
       - Workspace-level roles: user management, role management, and group management
       - Namespace-level roles: user management and role management

## Download KubeKey

Follow the steps below to download KubeKey before you upgrade your cluster.

{{< tabs >}}

{{< tab "Good network connections to GitHub/Googleapis" >}}

Download KubeKey from its [GitHub Release Page](https://github.com/kubesphere/kubekey/releases) or use the following command directly.

```bash
curl -sfL https://get-kk.kubesphere.io | VERSION=v2.3.0 sh -
```

{{</ tab >}}

{{< tab "Poor network connections to GitHub/Googleapis" >}}

Run the following command first to make sure you download KubeKey from the correct zone.

```bash
export KKZONE=cn
```

Run the following command to download KubeKey:

```bash
curl -sfL https://get-kk.kubesphere.io | VERSION=v2.3.0 sh -
```

{{< notice note >}}

After you download KubeKey, if you transfer it to a new machine also with poor network connections to Googleapis, you must run `export KKZONE=cn` again before you proceed with the steps below.

{{</ notice >}} 

{{</ tab >}}

{{</ tabs >}}

{{< notice note >}}

The commands above download the latest release (v2.3.0) of KubeKey. You can change the version number in the command to download a specific version.

{{</ notice >}} 

Make `kk` executable:

```bash
chmod +x kk
```

## Upgrade KubeSphere and Kubernetes

Upgrading steps are different for single-node clusters (all-in-one) and multi-node clusters.

{{< notice info >}}

When upgrading Kubernetes, KubeKey will upgrade from one MINOR version to the next MINOR version until the target version. For example, you may see the upgrading process going from 1.16 to 1.17 and to 1.18, instead of directly jumping to 1.18 from 1.16.

{{</ notice >}}

### All-in-one cluster

Run the following command to use KubeKey to upgrade your single-node cluster to KubeSphere 3.3 and Kubernetes v1.22.10:

```bash
./kk upgrade --with-kubernetes v1.22.10 --with-kubesphere v3.3.1
```

To upgrade Kubernetes to a specific version, explicitly provide the version after the flag `--with-kubernetes`. Available versions are v1.19.x, v1.20.x, v1.21.x, v1.22.x, and v1.23.x (experimental support).

### Multi-node cluster

#### Step 1: Generate a configuration file using KubeKey

This command creates a configuration file `sample.yaml` of your cluster.

```bash
./kk create config --from-cluster
```

{{< notice note >}}

It assumes your kubeconfig is allocated in `~/.kube/config`. You can change it with the flag `--kubeconfig`.

{{</ notice >}}

#### Step 2: Edit the configuration file template

Edit `sample.yaml` based on your cluster configuration. Make sure you replace the following fields correctly.

- `hosts`: The basic information of your hosts (hostname and IP address) and how to connect to them using SSH.
- `roleGroups.etcd`: Your etcd nodes.
- `controlPlaneEndpoint`: Your load balancer address (optional).
- `registry`: Your image registry information (optional).

{{< notice note >}}

For more information, see [Edit the configuration file](../../installing-on-linux/introduction/multioverview/#2-edit-the-configuration-file) or refer to the `Cluster` section of [the complete configuration file](https://github.com/kubesphere/kubekey/blob/release-2.2/docs/config-example.md) for more information.

{{</ notice >}}

#### Step 3: Upgrade your cluster
The following command upgrades your cluster to KubeSphere 3.3 and Kubernetes v1.22.10:

```bash
./kk upgrade --with-kubernetes v1.22.10 --with-kubesphere v3.3.1 -f sample.yaml
```

To upgrade Kubernetes to a specific version, explicitly provide the version after the flag `--with-kubernetes`. Available versions are v1.19.x, v1.20.x, v1.21.x, v1.22.x, and v1.23.x (experimental support).

{{< notice note >}}

To use new features of KubeSphere 3.3, you may need to enable some pluggable components after the upgrade.

{{</ notice >}} 
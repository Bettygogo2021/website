---
title: "Upgrade with ks-installer"
keywords: "Kubernetes, upgrade, KubeSphere, v3.3.1"
description: "Use ks-installer to upgrade KubeSphere."
linkTitle: "Upgrade with ks-installer"
weight: 7300
---

ks-installer is recommended for users whose Kubernetes clusters were not set up by [KubeKey](../../installing-on-linux/introduction/kubekey/), but hosted by cloud vendors or created by themselves. This tutorial is for **upgrading KubeSphere only**. Cluster operators are responsible for upgrading Kubernetes beforehand.

## Prerequisites

- You need to have a KubeSphere cluster running v3.2.x. If your KubeSphere version is v3.1.x or earlier, upgrade to v3.2.x first.
- Read [Release Notes for 3.3](../../../v3.3/release/release-v330/) carefully.
- Back up any important component beforehand.
- Supported Kubernetes versions of KubeSphere 3.3: v1.19.x, v1.20.x, v1.21.x, v1.22.x, and v1.23.x (experimental support).

## Major Updates

   Compared with previous versions, KubeSphere 3.3.1 has witnessed major updates of permission control. Platform-level built-in roles `users-manager` and `workspace-manager` are deleted and `platform-self-provisioner` is added. For more information about built-in roles, refer to [Create a user](../../quick-start/create-workspace-and-project/#Create a user). Therefore, before you upgrade KubeSphere to 3.3.1, please note the following:

   - Change of built-in roles: If a tenant has been bound to `users-manager` or `workspace-manager`, its role will be changed to `platform-regular`.
   - Change of user-defiend roles: As KubeSphere 3.3.1 has blocked some permissions of user-defiend roles. After you upgrade KubeSphere to 3.3.1, blocked permissions of user-defiend roles will be revoked, but user-defiend roles will be retained. Blocked permissions of users at different levels vary:
       - Platform-level roles: user management, role management, and workspace management
       - Workspace-level roles: user management, role management, and group management
       - Namespace-level roles: user management and role management

## Apply ks-installer

Run the following command to upgrade your cluster.

```bash
kubectl apply -f https://github.com/kubesphere/ks-installer/releases/download/v3.3.1/kubesphere-installer.yaml  --force
```

## Enable Pluggable Components

You can [enable new pluggable components](../../pluggable-components/overview/) of KubeSphere 3.3 after the upgrade to explore more features of the container platform.


---
title: "使用 ks-installer 升级"
keywords: "kubernetes, 升级, kubesphere, 3.3"
description: "使用 ks-installer 升级 KubeSphere。"
linkTitle: "使用 ks-installer 升级"
weight: 7300
---

对于 Kubernetes 集群不是通过 [KubeKey](../../installing-on-linux/introduction/kubekey/) 部署而是由云厂商托管或自行搭建的用户，推荐使用 ks-installer 升级。本教程**仅用于升级 KubeSphere**。集群运维员应负责提前升级 Kubernetes。

## 准备工作

- 您需要有一个运行 KubeSphere v3.2.x 的集群。如果您的 KubeSphere 是 v3.1.0 或更早的版本，请先升级至 v3.2.x。
- 请仔细阅读 [3.3 版本说明](../../../v3.3/release/release-v330/)。
- 提前备份所有重要的组件。
- KubeSphere 3.3 支持的 Kubernetes 版本：v1.19.x、v1.20.x、v1.21.x、v1.22.x 和 v1.23.x（实验性支持）。

## 重要提示

   和之前的版本相比，KubeSphere 3.3.1 权限控制有很大的变化。删掉了平台级内置角色 `users-manager`(用户管理员)和 `workspace-manager`（企业空间管理员），增加了平台级内置角色 `platform-self-provisioner`。关于平台角色的具体描述，请参见[创建用户](../../quick-start/create-workspace-and-project/#创建用户)。因此，在您升级到 KubeSphere 3.3.1时，请注意以下几点：

   - 内置角色升级：如果已有租户绑定了 `users-manager` 和 `workspace-manager`，他们的角色将会变更为 `platform-regular`。
   - 自定义角色的升级：由于 KubeSphere 3.3.1 屏蔽掉了自定义角色的某些权限项，升级到 KubeSphere 3.3.1 后，自定义角色会被保留，但是其包含的已被屏蔽的权限会被删除。不同级别角色被屏蔽的权限如下：
       - 平台级角色被屏蔽的权限：用户管理，角色管理，企业空间管理
       - 企业空间级被屏蔽的权限：成员管理，角色管理，组管理
       - 命名空间级被屏蔽的权限：成员管理，角色管理

## 应用 ks-installer

运行以下命令升级集群：

```bash
kubectl apply -f https://github.com/kubesphere/ks-installer/releases/download/v3.3.1/kubesphere-installer.yaml  --force
```

## 启用可插拔组件

您可以在升级后启用 KubeSphere 3.3 的[可插拔组件](../../pluggable-components/overview/)以体验该容器平台的更多功能。
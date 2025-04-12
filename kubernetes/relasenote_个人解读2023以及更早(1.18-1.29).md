

## 1.29

变化
1. SidecarContainers进入Beta，默认启用。此功能允许将init容器的restartPolicy设置为Always，使之成为一个Sidecar容器，支持独立启动、停止或重启，且不会影响主应用容器和其他Init容器。更多信息，请参见Sidecar Containers。


参考: 
1. https://kubernetes.io/zh-cn/blog/2023/11/16/kubernetes-1-29-upcoming-changes/
2. https://help.aliyun.com/zh/ack/ack-managed-and-ack-dedicated/user-guide/kubernetes-1-30-release-notes?spm=a2c4g.11186623.help-menu-85222.d_2_0_0_2.7fee4781SqtnFE#fe0ecf0287t2w

## 1.27,1.28

变化
1. PersistentVolume (PV) 控制器已修改为：当未设置 storageClassName 时，自动向任何未绑定的 PersistentVolumeClaim 分配一个默认的 StorageClass https://kubernetes.io/zh-cn/blog/2023/08/18/retroactive-default-storage-class-ga/


小修小补(仅仅是依据个人工作需要来判断而已)
1. 原地调整 Pod 资源 (alpha)  https://kubernetes.io/zh-cn/docs/tasks/configure-pod-container/resize-container-resources/
2. 原生边车容器: https://kubernetes.io/zh-cn/blog/2023/08/25/native-sidecar-containers/

参考:  
1. https://kubernetes.io/zh-cn/blog/2023/03/17/upcoming-changes-in-kubernetes-v1-27/
1. https://help.aliyun.com/zh/ack/ack-managed-and-ack-dedicated/user-guide/kubernetes-1-28-release-notes


## 1.25,1.26
变化
1. 对 cgroups v2 的支持进入 Stable 阶段
2. 核心 CSI 迁移为稳定版

参考: 
1. https://kubernetes.io/zh-cn/blog/2022/11/18/upcoming-changes-in-kubernetes-1-26/
2. https://kubernetes.io/zh-cn/blog/2022/08/23/kubernetes-v1-25-release/
3. https://help.aliyun.com/zh/ack/ack-managed-and-ack-dedicated/user-guide/kubernetes-1-26-release-notes?spm=a2c4g.11186623.help-menu-85222.d_2_0_0_4.7fee4781jDkeHJ


## 1.23,1.24
变化
1. 通过Dockershim对Docker的支持现已经移除
2. Secret API将不会为ServiceAccount自动创建Secret对象存放Token信息，需要使用TokenRequest API来获取ServiceAccount的Token，该Token具备过期时间，更加安全。如果一定需要创建一个永不过期的Token，请参见service-account-token-secrets。

参考: 
1. https://kubernetes.io/zh-cn/blog/2022/04/07/upcoming-changes-in-kubernetes-1-24/
2. https://help.aliyun.com/zh/ack/ack-managed-and-ack-dedicated/user-guide/kubernetes-1-24-release-notes?spm=a2c4g.11186623.help-menu-85222.d_2_0_0_5_0.197925815MxP90


## 1.21,1.22

变化
1. 1.21版本后，默认开启IPv4/IPv6双栈（IPv6DualStack）特性

小修小补(仅仅是依据个人工作需要来判断而已)
1. 1.21后,具有多个容器的Pod可以使用kubectl.kubernetes.io/default-container注释，以便为kubectl命令预先选择一个容器

参考:
1. https://help.aliyun.com/zh/ack/ack-managed-and-ack-dedicated/user-guide/kubernetes-1-22-release-notes?spm=a2c4g.11186623.help-menu-85222.d_2_0_0_5_1.631d2bcbtNmniq
2. https://docs.ksyun.com/documents/43233?type=3
3. 

## 1.19,1.20

变化
1. 从1.19,【弃用】Ingress和IngressClass资源的extensions/v1beta1 API和networking.k8s.io/v1beta1 API已经废弃，并且会在1.22版本之后被移除。请使用networking.k8s.io/v1替代。
2. 【弃用】对于专有版的Master节点标签，ACK默认使用node-role.kubernetes.io/control-plane，同时在后续版本（不包含1.20）废弃使用node-role.kubernetes.io/master。


参考:
1. https://kubernetes.io/zh-cn/blog/2020/12/08/kubernetes-1-20-release-announcement/
2. https://help.aliyun.com/zh/ack/ack-managed-and-ack-dedicated/user-guide/kubernetes-1-20-release-notes?spm=a2c4g.11186623.help-menu-85222.d_2_0_0_5_2.2a705845PwKAnt
3. https://www.ctyun.cn/document/10025153/10075933


## 1.18
变化
1. 【弃用】所有资源的API apps/v1beta1和apps/v1beta2都将弃用，请使用apps/v1代替。
2. 【弃用】Daemonsets/Deployments/Replicasets资源的API extensions/v1beta1将被弃用，请使用apps/v1代替。
3. 【弃用】Networkpolicies资源的API extensions/v1beta1将被弃用，请使用networking.k8s.io/v1代替。

参考: 
1. https://kubernetes.io/zh-cn/blog/2020/03/25/kubernetes-1-18-release-announcement/
2. https://help.aliyun.com/zh/ack/ack-managed-and-ack-dedicated/user-guide/kubernetes-1-18-release-notes?spm=a2c4g.11186623.help-menu-85222.d_2_0_0_5_3.2fe5761esYrBCm
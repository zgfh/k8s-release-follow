# 2024年k8s的变化

个人看法(仅仅是从个人工作需要视角的总结，非常片面): 
新功能:  最多的是 DRA 变化
加固: 很多 k8s 稳定性的加固,让 k8s 更加的健壮、易用、易维护


## 1.32

变化
1. DRA 增强: DRA是 Kubernetes 资源管理系统的关键组件，这些增强旨在提高对需要专用硬件（如 GPU、FPGA 和网络适配器） 的工作负载进行资源分配的灵活性和效率
2. 调度器: 使用 QueueingHint 改进 Pod 调度重试,提升调度器性能 https://kubernetes.io/zh-cn/blog/2024/12/12/scheduler-queueinghint/
3. 流式处理 list 请求: 之前operator watch资源是第一次用list查询所有，然后用watch 监听变化,但会导致operator和apiserver 失联后同时启动时，会产生大量list请求导致apiserver压力很大，新方案改为第一次查询所有资源也通过watch一个一个接收,这样就避免了这个问题(非常好的设计，点赞👍🏻): https://kubernetes.io/zh-cn/blog/2024/12/17/kube-apiserver-api-streaming/




小修小补(仅仅是依据个人工作需要来判断而已)
    1. pod status 增加了镜像拉起的更多信息 `status.containerStatuses.state.waiting`
    2. sts 增加了对pvc的处理策略，可以实现自动删除pvc,`spec.persistentVolumeClaimRetentionPolicy`
    3. CRD 支持 field-selector
    4. type: LoadBalancer 类型的 Service 引入了 ipMode 字段, 该字段可以设置为 "VIP" 或 "Proxy",使用 "VIP" 时，kube-proxy 会继续处理负载均衡，保持现有的行为。使用 "Proxy" 时， 流量将直接发送到负载均衡器，提供云提供商对依赖 kube-proxy 的更大控制权；
    5. 当创建资源设置generateName而不是name时，降低冲突导致失败的几率（generateName能力: 不像之前，创建资源必须设定name，可以设置generateName让apiserver 自动生成name,[demo](./releasenote_demo/1.32/generateName_demo.yaml)）
    6. 内存管理器升级到GA: https://kubernetes.io/zh-cn/blog/2024/12/13/memory-manager-goes-ga/
    7. cpu 性能优化,可以手动设定系统用那几个cpu进行绑核操作，提升性能: https://kubernetes.io/zh-cn/blog/2024/12/16/cpumanager-strict-cpu-reservation/


参考: 
1. https://kubernetes.io/zh-cn/blog/2024/11/08/kubernetes-1-32-upcoming-changes/
2. https://dbaplus.cn/news-134-6393-1.html
  
## 1.31

变化
1. 存储: 删除所有云驱动的树内集成组件,删除 CephFS 卷插件,删除 Ceph RBD 卷插件,kube-scheduler 中非 CSI 卷限制插件的弃用
2. 支持具有多个服务 CIDR 的集群在 v1.31 中晋级为 Beta 版(默认禁用): 用户可以方便的切换service的地址段了 https://kubernetes.io/zh-cn/docs/reference/networking/virtual-ips/#ip-address-objects
3. Cgroup v1 进入维护模式
4. 通过基于缓存的一致性读加速集群性能(简单来说就是提升了apisever的性能) https://kubernetes.io/zh-cn/blog/2024/08/15/consistent-read-from-cache-beta/
5. matchLabelKeys: 解决三个节点+三个pod反亲和情况下,无法更新的问题(非常好的变化👍🏻): https://kubernetes.io/zh-cn/blog/2024/08/16/matchlabelkeys-podaffinity/#matchlabelkeys-%E4%B8%BA%E5%A4%9A%E6%A0%B7%E5%8C%96%E6%BB%9A%E5%8A%A8%E6%9B%B4%E6%96%B0%E5%A2%9E%E5%BC%BA%E4%BA%86%E8%B0%83%E5%BA%A6
6. 支持把镜像当volume挂着到容器的某个目录(ai 场景挂载模型文件非常好用): https://kubernetes.io/zh-cn/blog/2024/08/16/kubernetes-1-31-image-volume-source/

小修小补(仅仅是依据个人工作需要来判断而已)
1. AppArmor 支持现已稳定( securityContext 的 appArmorProfile.type 字段)
2. kube-proxy 做了类似pod的优雅退出，使节点下线时可以做到先停止流量, 保证流量无损,https://kubernetes.io/zh-cn/docs/reference/networking/virtual-ips/#external-traffic-policy
3. kube-proxy 的 nftables 后端,默认启动
4. 存储: VolumeAttributesClass,用于设定pv那些自动可以修改 https://kubernetes.io/zh-cn/docs/concepts/storage/volume-attributes-classes/#the-volumeattributesclass-api
5. 基于选择算符的细粒度鉴权, 此特性允许 Webhook 鉴权组件和未来（但目前尚未设计）的树内鉴权组件允许 list 和 watch 请求， 前提是这些请求使用标签和/或字段选择算符。 例如，现在鉴权组件可以表达：此用户不能列出所有 Pod，但可以列举所有 .spec.nodeName 匹配某个特定值的 Pod
6. 通过 Pod 状态暴露设备健康信息: status.allocatedResourcesStatus
7. kubectl流式传输从 SPDY 转换为 WebSocket(这样让apiserver的流量更符合七层流量) https://kubernetes.io/zh-cn/blog/2024/08/20/websockets-transition/
8. 才发现kubectl debug node 已经实现了(👍🏻): `kubectl debug node/mynode -it --image=m.daocloud.io/docker.io/library/busybox `

参考:
1. https://kubernetes.io/zh-cn/blog/2024/07/19/kubernetes-1-31-upcoming-changes/
2. https://kubernetes.io/zh-cn/blog/2024/08/13/kubernetes-v1-31-release/
3. 持久卷最近阶段转换时间, 每个 PersistentVolume 对象将有一个新字段 .status.lastTransitionTime 保存卷最近转换阶段的时间戳。首次在各阶段（Pending、Bound 或 Released）之间转换时填充
4. PersistentVolumes 回收策略的变更, 避免一些特殊情况导致的pv残留
5. 自动询问CRI并选择一致的cgoup驱动: cgroupfs 或 systemd: https://kubernetes.io/zh-cn/blog/2024/08/21/cri-cgroup-driver-lookup-now-beta/
6. kubeadm 配置版本升级为v1beta4 https://kubernetes.io/zh-cn/blog/2024/08/23/kubernetes-1-31-kubeadm-v1beta4/

## 1.30

变化
1. 用户命名空间进阶至(之前的版本，容器内的uid就是主机上的uid，导致有些给予uid 的限制如ulimit等会计算错误) Beta https://kubernetes.io/zh-cn/blog/2024/04/22/userns-beta/

小修小补(仅仅是依据个人工作需要来判断而已)

参考:
1. https://kubernetes.io/zh-cn/blog/2024/03/12/kubernetes-1-30-upcoming-changes/
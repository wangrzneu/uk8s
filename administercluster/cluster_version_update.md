# 集群版本升级

本文主要介绍如何在 UK8S 集群中，使用版本升级功能。

> 大版本升级暂时仅支持大于等于 1.14 版本的 UK8S 集群

## 注意事项

1. 升级之前请务必关闭集群中的伸缩组插件！
2. 目前版本升级支持大版本升级和小版本升级。
3. 由于大版本之间 API 及组件变化较大，现大版本升级支持集群大于等于 1.14 版本进行使用。
4. 小版本升级支持升级至 UK8S 维护的最新小版本，具体可以点击版本升级进行查询。
5. 版本升级会导致集群内容器重启，导致重启会有以下几种情况：
   1. 1.14 版本集群进行大版本升级，升级至 1.15、1.16、1.17、1.18
   2. 1.15 版本集群进行大版本升级，升级至 1.16、1.17、1.18
   3. 单节点升级时间过长（kubernetes 中节点 NotReady 持续 5 分钟后，Pod 将会飘到其他 Ready 节点上）


## 升级操作

> 请在集群升级之前，在集群中所有节点创建拥有 Root 权限的账号且保证密码一致，关闭集群中的集群伸缩插件。

我们在集群列表页面，选择需要升级的集群，并点击操作【更多】按键可以看到【版本升级】。

![](/images/administercluster/cluster_update2.png)

点击【版本升级】后进入【UK8S 版本升级】界面：
![](/images/administercluster/cluster-upgrade-form.png)

在此界面中，您可以设置升级的目标版本、升级策略，并提供 SSH 账号及密码，是否强制升级等。

> 强制升级将会忽略 K8S 集群预检查，直接进行升级，仍需要输入正确的 Root 账号密码。

可选的升级策略有：

- 指数批量升级：使用指数批量升级将按每批 1，2，4，8，16，32 的数量升级节点，
由于集群中的应用的多个副本都部署在被升级同一批节点上，使用此升级策略有造成服务分钟级中断的风险

- 逐个升级：使用逐个升级每次只升级 1 个节点，节点较多时升级过程较长，请合理规划升级窗口

设置好上述选项后，按【立即升级】按钮进入集群升级操作进度界面。
集群升级操作较为耗时，请耐心等待。升级期间不要对集群节点进行操作，确保升级顺利完成。

![](/images/administercluster/cluster_update3.png)

升级集群耗时根据节点不同时间也有所不同，例如 10 台节点的集群约耗时 10 分钟。
升级过程您可以关闭该页面，并随时可以通过点击集群列表中的【版本升级】再次进入查看升级进度。

## 升级暂停

为了支持大规模集群升级操作过程中需要暂时停止升级，UK8S 控制台提供了【停止升级】的功能，
给用户提供了更多手段控制复杂的集群升级操作。
您可在升级过程中按下图所示的界面，点击【停止升级】按钮，暂停 UK8S 集群升级操作：

![](/images/administercluster/cluster-upgrade-pause1.png)

点击此按钮后系统将提示以下内容：

![](/images/administercluster/cluster-upgrade-pause2.png)

如果确实需要停止升级操作，请按【确定】按钮。系统将在当前批次的节点完成升级操作后，停止升级其它未开始的节点。
下图是集群升级停止后的界面展示样例：

![](/images/administercluster/cluster-upgrade-pause3.png)

根据上图，您可以观察到集群中的部分 master 节点已经升级到目标版本。其它 master 节点还是原来的版本。
如果您需要恢复升级，可以通过点击集群列表中的【版本升级】再次进入升级界面。

## 升级失败

如遇升级失败，可以在集群列表页面点击操作【更多】按键可以看到【版本升级】下载升级日志，
如是您在升级期间对集群有操作导致的失败您可以根据日志提示修改后，重新进行升级操作。

如您对于日志所述内容无法进行判断，请您与我们技术支持联系，对集群进行诊断后重新进行升级。

## 常见升级失败原因

- 集群节点处于关闭或 kubelet 运行状态不正常
- 集群节点无法正常 SSH，密码错误等
- 集群节点可以 SSH，但无 Root 权限
- 集群伸缩插件没有关闭，导致新加入集群的节点无权限操作

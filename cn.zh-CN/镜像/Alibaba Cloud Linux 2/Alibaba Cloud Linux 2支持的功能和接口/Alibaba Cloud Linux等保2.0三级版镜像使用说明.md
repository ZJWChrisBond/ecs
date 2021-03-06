# Alibaba Cloud Linux等保2.0三级版镜像使用说明

本文介绍Alibaba Cloud Linux等保2.0三级版镜像及镜像的使用说明。

阿里云根据国家信息安全部发布的*GB/T22239-2019信息安全技术网络安全等级保护基本要求*中对操作系统提出的一些等级保护要求，推出自研云原生操作系统Alibaba Cloud Linux等保2.0三级版镜像。您使用本镜像无需额外配置即可满足以下等保合规要求：

-   身份鉴别
-   访问控制
-   安全审计
-   入侵防范
-   恶意代码防范

更多信息，请参见[Alibaba Cloud Linux等保2.0三级版镜像检查规则说明](#section_6yo_suu_6ww)。

## 使用Alibaba Cloud Linux等保2.0三级版镜像

1.  创建ECS实例，并选择Alibaba Cloud Linux等保2.0三级版镜像。

    创建ECS实例的具体操作步骤请参见[使用向导创建实例](/cn.zh-CN/实例/创建实例/使用向导创建实例.md)。创建过程中需注意以下配置项：

    -   在**镜像**区域选择`Alibaba Cloud Linux 2.1903 LTS 64位 等保2.0三级版`镜像。

        ![dengbao image](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7246462061/p172961.png)

    -   由于等保合规的要求，等保镜像不支持使用密钥对的方式创建和远程连接ECS实例，只支持用户名密码的方式。因此**登录凭证**请选择**自定义密码**。
2.  远程连接ECS实例，并手动进行等级保护加固的配置。

    按照*GB/T22239-2019信息安全技术网络安全等级保护基本要求*，同时也为了不影响用户正常登录和使用，部分配置需要用户手动执行命令进行配置。用户可以通过用户名密码方式远程连接ECS实例后，执行`/root/cybersecurity.sh`脚本来实现等级保护的加固。

    1.  远程连接ECS实例。

        详情请参见[使用用户名密码验证连接Linux实例](/cn.zh-CN/实例/连接实例/连接Linux实例/使用用户名密码验证连接Linux实例.md)。

        成功登录ECS实例后可以看到以下说明：

        ![dengbao login](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7999382061/p174312.png)

    2.  执行脚本，完成镜像加固。

        执行脚本后，需要根据系统提示依次创建普通用户、审计员和安全员账户三个用户。

        1.  执行脚本。

            ```
            sh cybersecurity.sh
            ```

        2.  输入普通用户`admin`，并设置密码。
        3.  输入审计员用户`audit`，并设置密码。
        4.  输入安全员用户`security`，并设置密码。
        脚本执行的内容示例如下：

        ![dengbao site](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7999382061/p174315.png)

3.  使用系统。

    等级保护加固的配置，限制了root用户直接登录。您需要使用已创建的普通用户、审计员或安全员账户登录系统，然后切换至root用户使用系统。本步骤使用普通用户作为示例，介绍如何登录系统并切换用户。

    1.  使用普通用户`admin`远程连接ECS实例。

    2.  切换至root用户。

        ```
        su root
        ```

        输入root用户的密码即可成功切换用户。

    切换操作的示例如下图：

    ![result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7246462061/p172992.png)


## Alibaba Cloud Linux等保2.0三级版镜像检查规则说明

Alibaba Cloud Linux等保2.0三级版镜像按照*GB/T22239-2019信息安全技术网络安全等级保护基本要求*进行等级保护加固，满足对应的检查项。详细信息如下表所示。

|检查项类型|检查项名称|检查内容|
|-----|-----|----|
|身份鉴别|应对登录的用户进行身份标识和鉴别，身份标识具有唯一性，身份鉴别信息具有复杂度要求并定期更换。|-   检查是否存在空密码账户。
-   身份标识（UID）具有唯一性。
-   设置密码复杂度要求。
-   定期更换密码。
-   设置密码最短修改时间，防止非法用户短期更改多次。
-   限制密码重用。
-   确保root是唯一的UID为0的帐户。 |
|当对服务器进行远程管理时，应采取必要措施，防止鉴别信息在网络传输过程中被窃听。|-   检查SSHD是否强制使用V2安全协议。
-   禁止Telnet等不安全的远程连接服务。 |
|应具有登录失败处理功能，应配置并启用结束会话、限制非法登录次数和当登录连接超时自动退出等相关措施。|检测是否配置登陆失败锁定策略，是否设置空闲会话断开时间和启用登陆时间过期后断开与客户端的连接设置。|
|访问控制|应对登录的用户分配账户和权限。|-   除系统管理用户之外，应该分配普通用户、审计员、安全员帐户。
-   确保用户umask为027或更严格。
-   确保每个用户的home目录权限设置为750或者更严格。 |
|应重命名或删除默认账户，修改默认账户的默认口令。|-   Linux下root账号不应删除，检查是否禁止SSH直接登陆即可。
-   root之外的系统默认帐户、数据库帐户禁止登陆（non-login）。
-   确保无弱密码存在，对应的弱密码基线检测通过。 |
|访问控制的粒度应达到主体为用户级或进程级，客体为文件、数据库表级。|检查重要文件，如访问控制配置文件和用户权限配置文件的权限，是否达到用户级别的粒度。|
|应及时删除或停用多余的、过期的账户，避免共享账户的存在。|-   root之外的系统默认帐户、数据库帐户禁止登陆（non-login）。
-   锁定或删除shutdown、halt帐户。 |
|应授予管理用户所需的最小权限，实现管理用户的权限分离。|-   确保su命令的访问受限制。
-   检查/etc/sudoers配置sudo权限的用户，根据需要给root以外用户配置sudo权限，但除管理员外不能所有用户都配置（ALL）权限。 |
|应由授权主体配置访问控制策略，访问控制策略规定主体对客体的访问规则。|-   确保用户home目录权限设置为750或者更严格。
-   无主文件或文件夹的所有权，根据需要重置为系统上的某个活动用户。
-   设置ssh主机公钥文件的权限和所有权。
-   设置ssh主机私钥文件的权限和所有权。 |
|安全审计|应对审计记录进行保护，定期备份，避免受到未预期的删除、修改或覆盖等。|检查auditd文件大小、日志拆分配置或者备份至日志服务器。若自动修复失败，请先修复启用安全审计功能检查项。|
|审计记录应包括事件的日期和时间、用户、事件类型、事件是否成功及其他与审计相关的信息。|满足启用安全审计功能检查项，即满足此项。|
|应启用安全审计功能，审计覆盖到每个用户，对重要的用户行为和重要安全事件进行审计。|-   启用auditd服务。
-   启用rsyslog或syslog-ng服务。
-   确保收集用户的文件删除事件。
-   确保收集对系统管理范围（sudoers）的更改。
-   确保收集修改用户或用户组信息的事件。如使用了第三方日志收集服务，可自行举证并忽略此项。 |
|应保护审计进程，避免受到未预期的中断。|auditd是审计进程audit的守护进程，syslogd是日志进程syslog的守护进程，查看系统进程是否启动。|
|入侵防范|应能发现可能存在的已知漏洞，并在经过充分测试评估后，及时修补漏洞。|云安全中心的漏洞检测和修复功能可以满足。|
|应遵循最小安装的原则，仅安装需要的组件和应用程序。|应卸载NetworkManager、avahi-daemon、Bluetooth、firstboot、Kdump、wdaemon、wpa\_supplicant、ypbind等软件。|
|应关闭不需要的系统服务、默认共享和高危端口。|-   应关闭不需要的系统服务、文件共享服务。
-   关闭21 、23、25、111、427、631等高危端口。 |
|应能够检测到对重要节点进行入侵的行为，并在发生严重入侵事件时提供报警。|云安全中心有入侵检测和告警功能。如有其他方式，可自行举证并忽略此项。|
|应通过设定终端接入方式或网络地址范围对通过网络进行管理的管理终端进行限制。|/etc/hosts.allow文件指定允许连接到主机的IP地址，不应配为`ALL:ALL`；/etc/hosts.deny文件指定禁止连接到主机到IP，应该配置为`ALL:ALL`，默认禁止所有连接。 两者需要配合使用，且必须先配置`/etc/hosts.allow`规则。若是已通过其他方式实现如网络安全组、防火墙等，可自行举证并忽略此项。|
|恶意代码防范|应安装防恶意代码软件，并及时更新防恶意代码软件版本和恶意代码库。|检测是否安装使用云安全中心，如安装了其他防恶意代码软件，可自行举证并忽略此项。|


# 创建配备NVIDIA GPU的实例

配备NVIDIA GPU的实例必须安装驱动才可以使用GPU，涉及的驱动包括GPU驱动和GRID驱动。您可以在创建实例时设置自动安装驱动，也可以在创建实例后手动安装驱动。

完成创建ECS实例的准备工作：

1.  创建账号，以及完善账号信息。
    -   [注册阿里云账号](https://www.alibabacloud.com/help/doc-detail/50482.htm)。
    -   [绑定信用卡或PayPal账号](https://www.alibabacloud.com/help/doc-detail/50517.htm)。
    -   如果您需要购买中国内地地域的ECS实例，必须完成实名认证。更多详情，请参见[实名认证](https://www.alibabacloud.com/help/doc-detail/52595.htm)。
2.  阿里云提供一个默认的专有网络VPC，如果您不想使用默认专有网络VPC，可以在目标地域创建一个专有网络和交换机。具体操作，请参见[搭建IPv4专有网络](/intl.zh-CN/快速入门/搭建IPv4专有网络.md)。
3.  阿里云提供一个默认的安全组，如果您不想使用默认安全组，可以在目标地域创建一个安全组。具体操作，请参见[创建安全组](/intl.zh-CN/安全/安全组/创建安全组.md)。

如果您需要使用其它扩展功能，也需要完成相应的准备工作，例如：

-   如果创建Linux实例时要绑定SSH密钥对，需要在目标地域创建一个SSH密钥对。具体操作，请参见[创建SSH密钥对](/intl.zh-CN/安全/SSH密钥对/使用SSH密钥对/创建SSH密钥对.md)。
-   如果要设置自定义数据，需要准备实例自定义数据。具体操作，请参见[生成实例自定义数据](/intl.zh-CN/实例/管理实例/使用实例自定义数据/生成实例自定义数据.md)。
-   如果要为ECS实例关联某个角色，需要创建、授权实例RAM角色，并将其授予ECS实例。具体操作，请参见[授予实例RAM角色](/intl.zh-CN/安全/实例RAM角色/授予实例RAM角色.md)。

配备NVIDIA GPU的实例涉及以下驱动：

-   GPU驱动：用于驱动物理GPU。仅非vGPU的GPU实例支持安装GPU驱动。
-   GRID驱动：用于获得图形加速能力。配备vGPU的GPU实例（vgn6i和vgn5i）和非vGPU的GPU实例均支持安装GRID驱动，以获得GPU的图形加速能力。

GPU实例的驱动支持情况如下表所示。

|驱动类型|配备vGPU的GPU实例（vgn6i和vgn5i）|非vGPU的GPU实例|
|----|-------------------------|-----------|
|GPU驱动|不支持|支持|
|GRID驱动|支持|支持|

## 操作步骤

本步骤重点介绍配备NVIDIA GPU的实例相关的配置，如果您想了解其他通用配置，请参见[使用向导创建实例](/intl.zh-CN/实例/创建实例/使用向导创建实例.md)。

1.  前往[实例创建页](https://ecs-buy.aliyun.com/wizard/#/)。

2.  完成基础配置。

    **说明：** GPU实例在特定地域和可用区售卖，您可以前往[ECS实例可购买地域](https://ecs-buy.aliyun.com/instanceTypes/#/instanceTypeByRegion)页面查看。选择您需要的付费模式，输入实例规格名称搜索即可。

    vgn6i和vgn5i实例配备的是分片虚拟化后的虚拟GPU，只支持安装GRID驱动，请根据实例规格完成实例和镜像配置。

    -   创建配备vGPU的GPU实例（vgn6i和vgn5i）
        -   **实例**：定位到**异构计算GPU/FPGA/NPU** \> **GPU虚拟化型**，然后按需选择实例规格。
        -   **镜像**：操作系统类型影响GRID驱动的安装方式，如下所示：
            -   Windows：在镜像市场中搜索并使用收费镜像**Windows Server 2016 中文版预装GRID驱动**，该镜像带有已经激活License的GRID驱动，不用再手动安装GRID驱动。

                **说明：** 更多Windows Server版本的收费镜像即将上线。

            -   Linux：需要自行购买GRID License，并在创建实例后手动安装GRID驱动，请参见[在vgn6i和vgn5i实例中安装GRID驱动（Linux）](/intl.zh-CN/实例/选择实例规格/GPU计算型/在vgn6i和vgn5i实例中安装GRID驱动（Linux）.md)。
    -   创建非vGPU的GPU实例
        -   **实例**：定位到**异构计算GPU/FPGA/NPU** \> **GPU计算型**，然后按需选择实例规格。
        -   **镜像**：非vGPU的GPU实例支持安装GPU驱动，安装方式如下所示：

            **说明：** 如果您使用共享镜像和自定义镜像，需要自行保证安装了需要的GPU驱动和相关软件。

            -   设置自动安装GPU驱动。

                公共镜像是由阿里云官方或第三方合作商家提供的系统基础镜像，部分Linux镜像支持自动安装GPU驱动，如下所示：

                -   CentOS 64位（目前提供的所有自营版本均支持）
                -   Ubuntu16.04 64位镜像
                -   Ubuntu18.04 64位镜像
                -   SUSE Linux Enterprise Server 12 SP2 64位镜像
                -   Alibaba Cloud Linux 64位镜像
                如果您选择的镜像支持自动安装GPU驱动，选中**自动安装GPU驱动**复选框，并选择GPU驱动、CUDA、cuDNN库版本。如果是新业务系统，建议选择最新的版本。

                选中**自动安装GPU驱动**复选框后，您可以选择是否自动安装GPU云加速器。GPU云加速器提供了飞天AI加速器AIACC（Apsara AI Accelerator），可以帮助您快速搭建高性能分布式深度学习训练系统并加速AI训练性能，更多详情请参见[GPU云加速器](/intl.zh-CN/实例/选择实例规格/GPU计算型/GPU云加速器.md)。

                **说明：** CentOS 8、CentOS 6、SUSE Linux、Alibaba Cloud Linux暂时不支持GPU云加速器。

                对于支持自动安装GPU驱动的镜像，如果您清除**自动安装GPU驱动**复选框，仍可以在**实例自定义数据**模块下配置安装脚本，参考安装脚本请参见[自动安装脚本v3.1](#section_wrk_m0g_0zr)。

                **说明：** 如果调用RunInstances接口创建配备NVIDIA GPU的实例，必须通过UserData参数上传安装脚本，脚本内容需要采用Base64方式编码。

                ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0304359951/p39823.png)

            -   在镜像市场选择预装了GPU驱动和相关软件的镜像。

                镜像市场提供经严格审核的优质镜像，预装操作系统、应用环境和各类软件，无需配置即可一键部署云服务器。目前镜像市场提供了支持深度学习和机器学习的镜像：

                -   如果配备NVIDIA GPU的实例用于深度学习，您可以选择预装深度学习框架的镜像。在镜像市场搜索关键字深度学习并选择可用的镜像，目前仅支持CentOS 7.3。
                -   NVIDIA GPU Cloud VM Image（虚拟机镜像）是运行针对NVIDIA GPU优化的深度学习框架和HPC应用程序容器的优化环境。更多信息，请参见[在GPU实例上部署NGC环境](/intl.zh-CN/最佳实践/GPU实例最佳实践/在GPU实例上部署NGC环境.md)。
            -   在在创建实例后手动安装GRID驱动，请参见[手动安装GPU驱动](/intl.zh-CN/实例/选择实例规格/GPU计算型/手动安装GPU驱动.md)。
            非vGPU的GPU实例也支持安装GRID驱动。操作系统类型影响GRID驱动的安装方式，如下所示：

            -   Windows：在镜像市场中搜索并使用收费镜像**Windows Server 2016 中文版预装GRID驱动**，该镜像带有已经激活License的GRID驱动，不用再手动安装GRID驱动。

                **说明：** 更多Windows Server版本的收费镜像即将上线。

            -   Linux：需要自行购买GRID License，并在创建实例后手动安装GRID驱动，请参见[在GPU实例中安装GRID驱动（Linux）](/intl.zh-CN/实例/选择实例规格/GPU计算型/在GPU实例中安装GRID驱动（Linux）.md)。
3.  完成网络和安全组配置。

    在选择配置时，请注意：

    -   **网络**：选择**专有网络**。
    -   **公网带宽**：请根据您的业务需要选择带宽。

        **说明：** 如果您在**基础配置**中选用了Windows 2008 R2及以下版本的镜像，在GPU驱动安装生效后，您将无法通过管理终端连接配备NVIDIA GPU的实例，远程连接时会始终显示黑屏或停留在启动界面。您需要在此处选中**分配公网IP地址**复选框，或者在创建实例后绑定弹性公网IP，以便通过其他协议连接实例，例如RDP（Windows自带的远程连接）、PCOIP、XenDesktop HDX 3D等。其中RDP不支持DirectX、OpenGL等应用，您需要自行安装VNC服务和客户端。

4.  完成系统配置。

    在选择配置时，请注意：

    -   **登录凭证**：建议选择**密钥对**或**自定义密码**。如果您选择**创建后设置**，通过管理终端登录实例时必须绑定SSH密钥对或者重置密码，然后重启实例使修改生效。如果此时GPU驱动尚未安装完成，重启操作会导致安装失败。
    -   **实例自定义数据**：
        -   如果您在**基础配置**页面的**镜像**中选择了**自动安装GPU驱动**，此处会显示自动安装CUDA和GPU驱动的注意事项和Shell脚本内容。自动安装脚本已更新到v3.1版本。

            ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0304359951/p39825.png)

        -   如果您未选择**自动安装GPU驱动**，可以在**实例自定义数据**处配置安装脚本，脚本示例请参见[自动安装脚本v3.1](#section_wrk_m0g_0zr)。
5.  根据需要完成分组设置并确认订单，完成创建配备NVIDIA GPU的实例。

    如果您配置了自动安装脚本，实例启动后会自动安装GPU驱动。安装完成后实例会自动重启，重启过后GPU驱动才能正常工作。

    **说明：** GPU驱动在Persistence Mode下工作更稳定。安装脚本会自动开启GPU驱动的Persistence Mode，并将该设置添加到Linux系统服务中，开机自动启动服务，确保实例重启后还能默认开启Persistence Mode。

    自动安装过程受不同实例规格的内网带宽和CPU核数的影响，安装时间约10～20分钟，在安装过程中无法使用GPU，请勿对实例进行任何操作，也不要安装其它GPU相关软件，以防自动安装失败，导致实例不可用。您可以远程连接实例，通过安装日志查看安装进程和结果：

    -   如果正在安装，您可以看到安装进度条。
    -   如果已经安装成功，您可以看到安装结果提示**ALL INSTALL OK**。
    -   如果安装失败，您将看到安装结果提示**INSTALL FAIL**。
    -   详细安装日志位于/root/auto\_install/auto\_install.log。
    **说明：** 如果您在实例创建完成后更换操作系统，请确保使用支持自动安装CUDA和GPU驱动的镜像，避免自动安装失败。


## 自动安装GPU驱动脚本

实例首次启动时，cloud-init会自动执行Shell脚本安装GPU驱动、CUDA、cuDNN库。

-   如果您选中了**自动安装GPU驱动**复选框，可选的GPU驱动、CUDA、cuDNN库版本如下：

    |CUDA|GPU驱动|cuDNN|支持的公共镜像版本（仅支持自营镜像）|支持的实例规格|
    |----|-----|-----|------------------|-------|
    |10.2.89|440.64.00|7.6.5|    -   Alibaba Cloud Linux 2
    -   Ubuntu 18.04、16.04
    -   Centos 8.x、7.x、6.x
|    -   gn6v、gn6i、gn6e、gn5、gn5i、gn4
    -   ebmgn6v、ebmgn6i、ebmgn6e、ebmgn5i |
    |10.1.168|    -   440.64.00
    -   418.126.02
|    -   7.6.5
    -   7.5.0
|    -   Ubuntu 18.04、16.04
    -   Centos 7.x、6.x
|    -   gn6v、gn6i、gn6e、gn5、gn5i、gn4
    -   ebmgn6v、ebmgn6i、ebmgn6e、ebmgn5i |
    |10.0.130|    -   440.64.00
    -   418.126.02
|    -   7.6.5
    -   7.5.0
    -   7.4.2
    -   7.3.1
|    -   Ubuntu 18.04、16.04
    -   Centos 7.x、6.x
|    -   gn6v、gn6i、gn6e、gn5、gn5i、gn4
    -   ebmgn6v、ebmgn6i、ebmgn6e、ebmgn5i |
    |9.2.148|    -   440.64.00
    -   418.126.02
    -   390.116
|    -   7.6.5
    -   7.5.0
    -   7.4.2
    -   7.3.1
    -   7.1.4
|    -   Ubuntu 16.04
    -   Centos 7.x、6.x
|    -   gn6v、gn6e、gn5、gn5i、gn4
    -   ebmgn6v、ebmgn6e、ebmgn5i |
    |9.0.176|    -   440.64.00
    -   418.126.02
    -   390.116
|    -   7.6.5
    -   7.5.0
    -   7.4.2
    -   7.3.1
    -   7.1.4
    -   7.0.5
|    -   Ubuntu 16.04
    -   Centos 7.x、6.x
    -   SUSE 12sp2
|    -   gn6v、gn6e、gn5、gn5i、gn4
    -   ebmgn6v、ebmgn6e、ebmgn5i |
    |8.0.61|    -   440.64.00
    -   418.126.02
    -   390.116
|    -   7.1.3
    -   7.0.5
|    -   Ubuntu 16.04
    -   Centos 7.x、6.x
|    -   gn5、gn5i、gn4
    -   ebmgn5i |

-   如果您在**实例自定义数据**配置安装脚本，脚本内容请参见[自动安装脚本v3.1](#section_wrk_m0g_0zr)。

    自动安装脚本v3.1具有以下优势：

    -   提供最新版本的CUDA、GPU驱动和cuDNN库。
    -   登录实例后，如果正在安装驱动，您可以看到安装进度条。如果已经安装成功，实例会自动重启，重新登录后，您可以看到安装结果提示**ALL INSTALL OK**；如果安装失败，您将看到安装结果提示**INSTALL FAIL**。
    使用自动安装脚本v3.1时，您需要修改安装脚本的以下参数，指定GPU驱动、CUDA、cuDNN版本号，以及是否安装AIACC，如果不安装AIACC，则将IS\_INSTALL\_PERSEUS的值修改为FALSE，例如：

    ```
    IS_INSTALL_PERSEUS="FALSE"
    DRIVER_VERSION="440.64.00"
    CUDA_VERSION="10.2.89"
    CUDNN_VERSION="7.6.5"
    ```

    **说明：** 如果镜像是CentOS或SUSE操作系统，安装脚本使用.run安装包进行安装，如果镜像是Ubuntu操作系统，安装脚本使用.deb安装包进行安装。


## 自动安装脚本v3.1

```
#!/bin/sh

#Please input version to install
IS_INSTALL_PERSEUS=""
DRIVER_VERSION=""
CUDA_VERSION=""
CUDNN_VERSION=""
IS_INSTALL_RAPIDS="FALSE"

INSTALL_DIR="/root/auto_install"

#using .deb to install driver and cuda on ubuntu OS
#using .run to install driver and cuda on ubuntu OS
auto_install_script="auto_install.sh"

script_download_url=$(curl http://100.100.100.200/latest/meta-data/source-address | head -1)"/opsx/ecs/linux/binary/script/${auto_install_script}"
echo $script_download_url

mkdir $INSTALL_DIR && cd $INSTALL_DIR
wget -t 10 --timeout=10 $script_download_url && sh ${INSTALL_DIR}/${auto_install_script} $DRIVER_VERSION $CUDA_VERSION $CUDNN_VERSION $IS_INSTALL_PERSEUS $IS_INSTALL_RAPIDS
```

**相关文档**  


[RunInstances](/intl.zh-CN/API参考/实例/RunInstances.md)

[手动安装GPU驱动](/intl.zh-CN/实例/选择实例规格/GPU计算型/手动安装GPU驱动.md)

[在GPU实例中安装GRID驱动（Linux）](/intl.zh-CN/实例/选择实例规格/GPU计算型/在GPU实例中安装GRID驱动（Linux）.md)

[手动卸载GPU驱动](/intl.zh-CN/实例/选择实例规格/GPU计算型/手动卸载GPU驱动.md)

[GPU监控](/intl.zh-CN/主机监控/GPU监控.md)


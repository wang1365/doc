# 在vShpere上创建Ubuntu虚拟机

### 1. 登录vSphere Client
### 2. 新建虚拟机
> * 切换到Home > Inventory > Hosts and Clusters
> * 选择一个Host, 右键菜单选择"new virtual machine"
> * 在Configuration对话框中选择"Custom", 点击下一步
> * 在Name and Localtion中的name编辑栏输入虚拟机名字, 点击下一步
> * 在Storage中选择目的存储, 点击下一步
> * 在Virtual Machine Version中选择一个版本,我们这里选择"8", 点击下一步
> * 配置Guest Operating System, 选择要创建的虚拟机的操作系统, 我们选择Linux > Ubuntu Linux(64-bit), 点击下一步
> * 配置CPU, 例如2个socket和4个cores, 点击下一步
> * 配置Memory, 例如4GB, 点击下一步
> * 配置Network Connection, 采用默认配置, 点击下一步
> * 配置SCSI Controller, 采用默认配置, 点击下一步
> * 配置Disk, 选择"Create a new virtual disk", 点击下一步
> * 配置Disk size, 例如40GB, 点击下一步
> * Advanced Options, 采用默认配置, 点击下一步
> * 配置完成, 点击"Finish"

### 3. 配置ISO镜像

> * 在新建的虚拟机上右键菜单选择Edit Setting, Hardware > CD/DVD drive1 > Datastore ISO file > Browser,
选择一个ISO镜像, 例如选择fas3220a_vol1 > software > ubuntu-14.04.2-desktop-amd64.iso; Detail Status勾选
Connect at power on, 点击OK, 完成修改

### 4. 启动虚拟机, 安装Ubuntu
> * 在新建的虚拟机上右键菜单选择Power > Power On
> * 在新建的虚拟机上右键菜单选择Open Console打开远程桌面, 开始系统安装
> * Welcome界面, 语言选择English, 点击Install Ubuntu
> * Preparing Install Ubuntu, 点击Continue
> * Installation Type, 选择Erase disk and install Ubuntu, 点击Install Now > Continue
> * 时区选择, 选择Shanghai, 点击Continue
> * Keyboard Layout, 默认配置, 点击Continue
> * 用户创建, 填写要创建的用户及其密码, 点击Continue, 安装开始
> * 安装完成后会弹出对话框提示重启, 点击Restart Now重启虚拟机
> *
> *

### 5. 安装SSH server
> * 执行`sudo apt-get update`更新软件包
>   如果遇到`"Could not get lock /var/lib/apt/lists/lock ..."`错误, 有可能系统的Software Updater正在或
已经下载了更新包, 这样的话直接用Software Updater去安装更新包
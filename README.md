 # CentOS 7 模板报错的解决方法

- CentOS 7 已经达到其生命周期终点 (EOL)，官方删除了原 mirrorlist 子域名的解析，这导致了使用 CentOS 7 模板的用户在尝试更新或安装软件时可能会遇到如下报错：
```code
Failed to synchronize cache for repo 'base', disabling.
Failed to synchronize cache for repo 'extras', disabling.
Failed to synchronize cache for repo 'updates', disabling.
```
- 这是因为默认配置的镜像列表已经不再可用。要解决这个问题，可以采取以下方法之一：
1. 使用 Vault 存储库
  CentOS 提供了一个名为 Vault 的存储库，专门用于存放已达 EOL 版本的所有软件包。可以修改 yum 配置文件来使用这个存储库。

  编辑 /etc/yum.repos.d/CentOS-Base.repo 文件：
  ```code
  sudo vi /etc/yum.repos.d/CentOS-Base.repo
  ```

  将内容替换为以下配置：
```code
[base]
name=CentOS-$releasever - Base
baseurl=http://vault.centos.org/7.9.2009/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

[updates]
name=CentOS-$releasever - Updates
baseurl=http://vault.centos.org/7.9.2009/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

[extras]
name=CentOS-$releasever - Extras
baseurl=http://vault.centos.org/7.9.2009/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```
  保存文件并退出编辑器。

  
  清理 yum 缓存并更新：
```code
sudo yum clean all
sudo yum makecache
sudo yum update
```

2. 迁移到其他受支持的系统
  由于 CentOS 7 已经不再维护，建议迁移到其他受支持的操作系统，例如 CentOS 8、Rocky Linux、AlmaLinux 或其他兼容的企业级 Linux 发行版。

 
通过以上方法，可以暂时解决 #CentOS7 镜像列表不可用的问题，但长期来看，建议尽快迁移到受支持的操作系统以确保系统安全和稳定性。

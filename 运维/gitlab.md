# linux配置

```bash
systemctl stop firewalld	//关闭当前正在运行的防火墙
systemctl disable firewalld	//禁用开机启动的防火墙
vi /etc/sysconfig/selinux 	将	SELINUX = disabled	//防止阻止对gitlab的访问
reboot
```

# 安装gitlab

```bash
yum -y install curl policycoreutils openssh-server openssh-clients postfix	//git组件
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash	//gitlab仓库源

```


# 作用

- 持续部署，持续性的将模块进行部署服务器测试
- 持续集成，持续性的将各个模块集成检测状态
- 持续交付，快速进行版本的迭代，让用户体验功能



- 降低更显
- 减少重复的过程
- 任何时间任何地点都可以生成可部署的软件
- 建立团队对产品开发的信息

# 原理

工作人员将代码推送到完整代码库会触发版本控制服务器的钩子函数，通知，jenkins调用GitSvn获取源码，调用Maven插件进行打包，调用 Deploy to container插件将war包部署到Tomcat服务器

# 插件

系统管理		->		插件管理	

rebuilder

用于构建，可以少输入很多参数 

safe restart

安全重启

# 基础设置

## 全局安全性

系统管理		->		全局安全配置		->		授权策略，安全矩阵	->添加admin，最右给全部权限

系统管理		->		用户管理		->	添加用户	->可以授权不给Administer管理系统权限

# Jenkins + SVN


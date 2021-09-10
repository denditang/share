## git如何配置多个ssh-key


### 背景

现在一般公司都会有自己私有的git

* 自己的公司私有的git，用于公司内部的工作开发
* github，日常大家经常用的，用于参与开源或者沉淀自己内容的源

所以需要在一台电脑，配置两个ssh-key，让电脑可以同时push到两个环境

### 方案

* 生成一个公司用的ssh-key  

```
ssh-keygen -t rsa -C 'xxx@company.com'
```




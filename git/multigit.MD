## git如何配置多个ssh-key


### 背景

现在一般公司都会有自己私有的git

* 自己的公司私有的git，用于公司内部的工作开发
* github，日常大家经常用的，用于参与开源或者沉淀自己内容的源

所以需要在一台电脑，配置两个ssh-key，让电脑可以同时push到两个环境

### 方案

* 1 生成一个公司用的ssh-key  

```
ssh-keygen -t rsa -C 'xxx@company.com' -f ~/.ssh/company_id_rsa
```

* 2 生成一个github用的ssh-key

```
ssh-keygen -t rsa -C 'denditang@github.com' -f ~/.ssh/github_id_rsa
```

* 3 在~/.ssh目录下新建一个config文件，用于配置不同域名启用不同的ssh-key


```
# gitlab
Host gitlab.****.****
HostName gitlab.****.****
UseKeychain yes
AddKeysToAgent yes
IdentityFile ~/.ssh/company_id_rsa

#github
Host github.com
HostName github.com
AddKeysToAgent yes
IdentityFile ~/.ssh/github_id_rsa

```

* 4 添加公钥到各个git源
将对应的公钥github_id_rsa.pub添加到各个环境的ssh key中，就可以愉快的一台电脑登录使用多个ssh-key了。


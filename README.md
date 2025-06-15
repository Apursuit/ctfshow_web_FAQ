# ctfshow_web_FAQ

这是一个ctfshow平台web方向的QA答疑（非官方），收录新手小白常见问题，减少群内师傅重复答疑的情况。

新手师傅在提问时，建议使用截图，而非拍照，截图完整一些，带上完整的问题描述，这样你更可能及时得到想要的回复，完整一些的页面信息可能没有你想象那么严重。

一个比较推荐阅读的文章：[提问的智慧](https://topstip.com/wp-content/uploads/2024/06/%E6%8F%90%E9%97%AE%E7%9A%84%E6%99%BA%E6%85%A7.pdf)


### 无回显RCE/dnslog是什么

Q：我在做命令执行没有结果，什么是无回显RCE？看到有师傅提到dnslog，什么是dnslog？

A：[无回显RCE/dnslog是什么？怎么利用](./无回显RCE-dnslog.md)

### web 50 shell环境的解释问题

Q：很多师傅在前面几题的铺垫下，会尝试这个命令，发现不能正常执行，提示文件不存在

A：[web50 重定向符与通配符一起使用时shell环境的解释问题](./web50重定向符通配符共用问题.md)

### 命令执行 反弹shell 监听不到

Q：一个常见问题，新手师傅在攻击机kali里开启监听，靶机反弹shell，kali监听不到

A：[反弹shell失败](./反弹shell失败.md)


### xss拿不到cookie

Q：我xss payload怎么拿不到cookie里的flag，什么是叉自己？

A：[xss拿不到cookie](./xss拿不到cookie.md)

### php短标签里反引号执行命令直接显示

Q：有师傅问，为什么短标签里反引号执行命令会直接显示在页面？<?=`ls`?>

A: 短标签`<?= ?>`是`<?php echo ?>`的简写形式，会直接输出标签内里的内容

官方文档：[PHP 标记](https://www.php.net/manual/zh/language.basic-syntax.phptags.php)


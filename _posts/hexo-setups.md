---
title: hexo setups
author: MoonXu
description: 懒癌犯了，摘要同标题~
comments: false
sticky: '21'
tags: []
password: ''
categories: []
abbrlink: 65109
date: 2022-02-04 14:30:00
---



### centos 关闭SELINUX

```shell
$ setenforce 0
$ sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config
```



### 创建tags、caterories

```shell
$ hexo new page "tags"
```

修改source/tags/index.md

```yaml
---
title: tags
date: 2017-07-10 16:36:26
type: "tags"
---
```

修改themes/next/_config.yml

```yaml
menu:
home: /
archives: /archives/
categories: /categories/
tags: /tags/
```

tags 不存在多级,cat可以多级

```taml
categories:
- [Sports, Baseball]
- Baseball
tags:
- Injury
- Fight
- Shocking
```

使用katex

```shell
$ npm un hexo-renderer-marked
$ npm i hexo-renderer-markdown-it-plus
```

### 修改自定义样式

`css/_common/outline/footer/index.styl`

```css
.footer-inner > div {
  padding-left: 8px;
  padding-right: 8px;
}
```

### 博客加密

https://github.com/D0n9X1n/hexo-blog-encrypt/blob/master/ReadMe.zh.md

```shell
$ npm install --save hexo-blog-encrypt
```

#### 对博文加密

```yaml

---
title: Hello World
tags:
- 作为日记加密
date: 2016-03-30 21:12:21
password: mikemessi
abstract: 有东西被加密了, 请输入密码查看.
message: 您好, 这里需要密码.
wrong_pass_message: 抱歉, 这个密码看着不太对, 请再试试.
wrong_hash_message: 抱歉, 这个文章不能被校验, 不过您还是能看看解密后的内容.
---

```

#### 对tag加密

```yaml

# Security
encrypt: # hexo-blog-encrypt
  abstract: 有东西被加密了, 请输入密码查看.
  message: 您好, 这里需要密码.
  tags:
  - {name: tagName, password: 密码A}
  - {name: tagName, password: 密码B}
  wrong_pass_message: 抱歉, 这个密码看着不太对, 请再试试.
  wrong_hash_message: 抱歉, 这个文章不能被校验, 不过您还是能看看解密后的内容.

```
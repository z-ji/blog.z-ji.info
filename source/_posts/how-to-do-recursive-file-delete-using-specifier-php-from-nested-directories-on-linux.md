title: "How to do recursive file delete using specifier (*.php) from nested directories on linux?"
tags:
  - linux
  - sys admin
id: 40
categories:
  - code snippets
date: 2012-04-17 09:03:20
---

You can use a command something like the following to do a recrusive delete.
```
find /home/zji -name \*.php |xargs rm
```
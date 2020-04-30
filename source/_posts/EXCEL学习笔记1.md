---
title: EXCEL学习笔记1-AND与OR的用法
date: 2018-10-05 23:16:43
updated: 2018-10-05 23:16:43
tags: EXCEL
categories: 编程语言
---

```bash
=IF(AND(AC2<AD2,Y2<AB2),"Y","N") # 套用IF语句，如果AC2<AD2且Y2<AB2，则判断为Y，否则为N，注意，Y或N要加引号，否则报错
=IF(OR(AE2="Y",AF2="Y"),"Y","N") # 套用IF语句，如果AE2="Y"或AF2="Y"，则判断为Y，否则为N
```
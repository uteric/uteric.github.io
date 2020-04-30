---
title: R语言学习笔记-赋值符号（<-与=）
date: 2020-02-06 21:09:51
updated: 2020-02-06 21:09:51
tags: R语言
categories: 编程语言
---

在R中，运算符<-和=是等价的，但是，赋值符号<-更加科学，它可以使用在任何地方，不会引起歧义；

```bash
The operators <- and = assign into the environment in which they are evaluated. The operator <- can be used anywhere, whereas the operator = is only allowed at the top level (e.g., in the complete expression typed at the command prompt) or as one of the subexpressions in a braced list of expressions.
```

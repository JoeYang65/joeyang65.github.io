---
title: "欢迎来到 Blowfish"
weight: 1
draft: false
description: "Discover what's new in Blowfish version 2.0."
tags: ["new", "docs"]
series: ["Documentation"]
series_order: 1
date: 2026-01-15T13:18:50-07:00
---

# Welcome页面
xxxx
index md
日期格式1
```
{{ $t := time.AsTime "2023-10-15T13:18:50-07:00" }}
{{ time.Format "2 Jan 2006" $t }} 
```


日期格式2
```
{{ $t := "15 Oct 2023" }}
{{ time.Format "January 2, 2006" $t }}
```

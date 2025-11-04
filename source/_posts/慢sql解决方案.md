---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632522POX%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBk7mENUkv8k%2B99LF5ud8wfDnI%2FhEAJ3tLpc7dphlvH6AiA7R1aOJq5r7dM4y35V4yp8BVPfS0qI%2B8mDTT9EkFUNFCr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMP9X99%2B4yBZV7ReWmKtwDSODNHSEietHcqeeJpgFBvHKn017YIZWaaqaKfumPh6jvy3h8xb%2B0OlM20kvkLwGfx%2BwcSunI3TMtRyVCKV0Qtze7VNAM1ZloEPHgpeNfv7oV41pkntsHvqN2vTX3fAm0I109mSVCWFkCIe9Y67PKeuC6S8QFRHLs2GXBQKpcFVfB1eQpyEa%2BtiOco%2B6arX5meD9JODObM43AMJJLvBrwIV7RvsxX9L4MVBvilYPQxg4sBh13AtMDR%2FEbKauQ2SJ0UGqMjQeXfj3Gs26S1kNIcaYkVMfi0tZDBDX5cPdpjp54%2FBJQq9yMq6KzssQl%2BrzFylYqau%2FQK3o%2Fbbj0usJw9xz7ESgJ78w7eKF75cliQHZgePsUVewlV9eb0OgfwgrDrncp%2BzmsueV1vJlx10mMcndVys4%2F2XUrnk2SRJd3MmHcBRvoxudxMzAW1h4iKrIURctsbOIdkH6y99U%2FlbVt5fPrW8NvWSi2wSPk7qwcMRQqIR4xY%2F%2BEIBDt%2BLPMebpBaOFNkgrei64Rav6avT%2F57PuY8FgF2s6EZTZybrnIkq4FB5nI4m0WbM1mIfmqIesUNoLwW3%2FfQYF0iDZYx5UfYupBp3OhdQ5fr%2F4k6A%2Fb%2BGd7FSmIR6FxODPUTf8w%2B6mmyAY6pgFRMhMLXM%2FF4uJ7U3oXIaOm4pCMrWKS26DDTFddbN3VoAs%2FnewTrurvIzS27dkj71dwrpns84ugby3W1BtGTeUMbMlRC8FdzCvSsUZGPmdG9vXInyi%2FaGdr9SeWQM0pnac6iZ9AFthj%2FQlBJEYtcrr98Ov0gHPUHewugPWJfjKZVRL5u4w0BJTCoDehBXzgwOTiO2GtmfIkTmgddChGSlxJ373srQTI&X-Amz-Signature=62fce8725065c15c5edba5f03ca60c5bab5d9821a97d1fa2881b1035d2084520&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


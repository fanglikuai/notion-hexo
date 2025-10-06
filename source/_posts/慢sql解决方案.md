---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UIV2EQK7%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFZ2vLUgFCDpncRsV13SuVnl1FkyEg4vFXYJ2NDBLWHRAiEAoAh9xKcpiMR9EzZdFhWxV20YYBQ0IzgbXSSmKx1rqOgqiAQIl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL%2BlmowX2Z7kSjMaTCrcA5xS3KhzX%2FT1mzMZ4OtjPQwrJg2w2V7HM4%2FegGZ4b3AysC2kiXvel7T2etgSdvBnRIZrKJpc%2Bdky%2B%2FersRtN7r6gYJDjmQ00MnPePDB6%2BYvRqoiKRHpuYdTx67HjCy8JLrDNHX%2B7MADF79yfkSTCKDC1AtFgofUrZEMH6Q%2BxavBIym0CAwMqIIIL26FZuWbROnrwTc%2BmFUzfXga4TaCxYAIrNismZwPY1t3vDE7H5dBM7PeZtVMEq0WHPv7dhKN85vUGSc1Z%2B7e%2BIJ1we%2BK0dIbMd4ICcsBsY2sdIgoUL%2BkcjhJc7Wl%2FZ9TWjrwvipaxZFP%2F3N3F%2Bof8tmG7AdMqYUxmVWfvFdAerdC%2BpzXjnlnVR85QFVLccrRkB%2B08H6ACecddZ%2F%2B1u6peEgZBgEt5fPNi9Uul6viKrMlFXWdd66tCJ8PDysskrRjLdsTgwjt4%2F5OHMVB4CkP%2FOqyod1oMEZbH1%2BsYJXFeHbMSa%2By1zlxlrEEHL%2FGIaAmpGodyuZP01HjM%2F%2FELBsqbd%2BQK4wiRH3doH1hrUL8wjs655Zc7WZBeKz79viZsYIXBBfufdP7UKyIaUQC29kmUBkfASmDTt22xlgDp%2FCastZrvTZ8EqVONROlU0HC50Vk4rXPrMPb0kMcGOqUBP2tc1QtTTrYGUSgNdlZ2guif1JxreTBlAgLCDqjnOzAOkBlVeyqVC5e8a%2B9AcOWxlJ4%2BykyXNLlLcvF63ZA2soNV16QNO2z%2BpfIZ6ouZnDSlLSn9Dtiks4kTEScXB70r2DTN7eq346vxw%2BhtTrCdp4KtgrvfUmfuQ4E1NWHlVG2nO06tEPpGsuMrGzy90bpfwkn5dglfGPyVilH57aNSXYbWlreQ&X-Amz-Signature=75102a9d0453150deefe5575ed06ee9b9107483470b0a5cdd03e27749a3306ce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WFFVRPQ5%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCUImIznjk18P0OLHKvoNLIGL8xZgfGzT%2BOS8iDAOwxVAIhAO3ptfwsGoU6PA4FQ1WpEou6eZL%2FR25AeuIpAhonYoV1Kv8DCHoQABoMNjM3NDIzMTgzODA1IgyxF4QB8XMevybRmHgq3AMOFK4N6BCcmJgovlJmWHNsXEEsM7SKCUDxmgxzP0vRozzYAvTHpN9ZntdOXjgV28De2JqSCQwfRajWlQfPfyN49gJDvdt0mYdZ9BiECqeijiBl9zEN3%2Bd5Zpn4BvB9az2%2Fwsv03XwMsvrb4nizl0xsLBGjRSt%2BbyoChCfWmsSzVbOyEZy5im67ueHYdZfuLifkJM1o1h0M0PUT7kN%2BhrU3w1Kwn8Cx7T%2FmeyXTLuyCu%2FdTk5csCOHpoYoRRTPN8C5EZ8uK5PSQ1C6CqfRvOs%2FCFntdYqiIjfuxJpSOLQlbNXlbZ9hNtzWGyiAqhYkaE00VHhA%2FbJYJpmRLr4BZRCEhDIrEa0Kuuwy%2F2tIdt%2BX5p4xY8H4ZKHvWyUSe7H3dlDmTXrxrPx5%2FEPad98CHG2v2eErHWuoU0JuW%2F1h%2BRzC7QD217Mq0%2BV0reiWS%2FihyP%2BjEan4e7d8r6RFjjniBAKn8k5Q7Qm91H3sa6yLDsMWjFmFhQPZX%2Br6k5CpIhvwmUSIVFtF1FROEOnZ%2BUaAV0bdDa15MWx51vEymn8blkYQjkZcBWNUPi4fCnjfwY%2BikJtLkyyVsZLJLyzQOXlu%2F4RO3blotFp%2BMxcBpE%2BA%2FZe20Zr4ZbBBol2WXWg39xTC%2BwIrHBjqkAT3tiFqvnRYlHIKuX4zNbaDEC%2FL6HVXPMOdgtBvRXJIf%2FsQGRDhSuJSHqbo%2BeHsngLM28wLhbTqzC9RW%2B%2F%2BPq5PcsdJN0RwtMVxOa3aYXAaYkHhRgT55Yylfc1A6VW49SsvejBbSntTkwrlRVidjBoDOblsnBJYIcuGnq%2FHtQrSYszYXhw9DfjstOLH3BLuvx%2BAsbUFyZvRMwKVn8RQ0IsF15pir&X-Amz-Signature=22a7d684cfafbee7d9f6e9d9d7b69cde84765bc0a01a20ee21c51779a3c08cd3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


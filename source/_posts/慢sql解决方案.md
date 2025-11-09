---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDBANFEY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T220036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJIMEYCIQDiyAuXe9AIZVHoMgY%2BL7KUBmeS4Z0r0c7Dt6ppC1DFmQIhAJD%2B9pzaVXglPw5PC%2FFCgMtxhoG0%2FMQP2FETT1HuYlxmKogECPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyHqkNV%2B0xaLW6ItrYq3AOJNZ3uQLGqvSrJKp7o7nsyLRQ3fS47fDr%2BV5hVM1hDrkQHKdFV0jyGJUnrPZup6%2BohsAngUnIKN8hP2PPqHHlUtdQbk9tKe39eoOc0BXj%2BINJUuhUULxciKbsPE0DEbVGOBSKhvNWXJ00Qtet2JoH4jcwXRdwvSozHC6DmOcF0pObn45%2FTrsXppavnVk09Y2TYTZcOznt%2F5OL%2B2slhsjBo3kG2vPCoE4OxIWfo%2B0HowvYEewcEE2OAEEgooVD695CHdDjWlDsh883vn1KQPdwSXpf6l8myMjawWLcI8MFC4bNXVum3Gcp3D3%2Biju5%2BgR0dDhinHVTY0kzAPMedqEiH9H26MVL7bowUIPiY0yaTq34gCI1DaVslD9XZDN%2BQCG4r%2FLLCNBxaS3P%2Bu%2B%2Fv033pnhZEaXW6keIpmIZcJriABUWXtOLtHrRgezyqUx0jqp%2FJlZQ7qYM0qXLO2VekXKsVMkvL7PXw0kVsYIcBsAPXJAn2np2MLvi49axRqddyNIKpwEH6Z%2BW%2FxaDCgQFnrWGO1F95hsuIPuV4pXvrUJV7C3wjj5fpRJlswBdLGjhVLEvdwj3XUoxKruX4nsZgIsfQGMh1kVnlDvLaiMG4ZxnWNWotRDKsL0f%2BvcjpDzDPgMPIBjqkAejCF4iD%2BwJQ%2FJsV8iAC7XKO%2Fj9cEdXAb4t3krfYN%2F%2Bz2zxWV5XrljwxKCRYHei%2BL04SaulRPeWZYukt8zSypkbVXqe%2FLPTg6iJfzRFYTqZvgYrnNP0hXnVwV6JmPvDuLuZ8UTZ7Ihy5uecVNz%2BnjbWD2LjaLH35bbabck48fHhJeHYgae4TwePnzQfguM%2Fb2jczOk6RUUQrI5YERdwPdeNCnVw2&X-Amz-Signature=4d0082e4ed5ed87da32fa1a88fa409f7ae3c2e0f9f43fcb7e57639c675440979&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


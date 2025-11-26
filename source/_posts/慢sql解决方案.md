---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664XNIOGIF%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC7sCXU%2F7MzDiR0bMrWs5rJCr57gTFCfDudCVB1Q5zYJQIhANpE0%2BJp0H%2BWcMWeWuU8eybSJszdRgERHSYmOndf4QfYKogECI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzmnKCnRWTJ%2BD%2BRJBkq3ANvaCSh90VqN8GfXNmpMQvsv7dHqESJBy5iGILMsx6SYsvqRXCVQwhFRDVoVHI6jMiVlVPM%2BMR7jRTPGD9STGTF9fAjA5I3sCE6WPxpr8YvN%2FKpHzKrT5Jt6H8vj023i5qHoS%2BePsHt9YDFxQhkG2EYLXvEiRHLxvcxDYirFzA%2F7xuZOKMRAv6zpesuUOB5njwNY0a1E1OwqkVu%2F0y7tS%2FOg4cyDPZ1vMpwrghRTD9%2Bsg9Q0HfgfdDtkfD7o7dIiclspr%2BIjuXUPvjLN266uTa9E8bq4I5QqZ0OcUMOkwoV%2FfozC%2FN7IDv57cuhd%2BcBSEwb7G47Oer%2BqD3JgZGH9Hr7M1kPbuIwThPCf3uM6NcbaGB5%2BcEuLN3XE5IQvBl%2FuKX%2FKJDjIP%2Bw6981RXzPITPMDABkOoPfj0JbLvPQGrDNaxbH3U%2BzF1y1DFnvW9ag32suhnzPbnGpbKtPc1qcnWikyOgIKQJOsm1p5iXcE7OinKlyJLe2QiDjQr%2FCscChrng5%2BETfJwrms39vuaY%2B7pIOQNeYAC%2Fm45oswWT0RFQilo9i%2FqiopK%2Bv%2FblZVUX0zGZMEC%2BjJDkTqS5VNtzFZbEA9uIPCx71NODq8%2B2GClHxY09Sgvr02MZQmBfNDjCjxZ3JBjqkAdh0YC%2F3QGeAszjO5sSA%2FYynxlMVQW7PmZNYODs%2B2strsW52l%2FC1b3I5lGL7SEeoCDfRlSlQoHpEOvw3KzEdx2M1fK9aaRqAtBnWDUEr2gecsp9cBUoRuGUwqKKKGv4aD9fEgOed0k2JkCGVlFMm%2B2AHu4StgadOuD%2BmoJXV70bSk6X97SfSmjUMk8MUhVnDsW9pfvCW929C5A49PBAhY7qrojfU&X-Amz-Signature=2a4b0b3cd41cfd1a1deb6c81583068dd0f175c893699f5ac0de7bc77f154e5c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


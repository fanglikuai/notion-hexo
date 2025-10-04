---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627HB6MT6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDRCXQYcQGWBQoQr%2FiMvkgolBNozJocGhf1vHFCls01%2FQIhAOqJmhVSxjMPUWsmGRKBsg%2FJzN4GZ481gkwLWnvM03mRKv8DCGEQABoMNjM3NDIzMTgzODA1IgxQMCe3X%2F%2Bi2Fuit5gq3AN5ZXz0DovJY9O8gmQEhb%2Fk3RCcvBLyxS%2BkNtNNojmwVvk0kbSeG0sx%2FN3FKkh0fAQufFzD9csiC%2Fzm%2FpfR6wVVfpUkeBESqSXaD7c3Yhwhw5iFop6wPuZeSXy%2FMejJEoNQCmp%2F4EjMsuAaZ9xfLi755kV8%2FIoypGYFfn6Qn0oS3%2FPEvqFngXipco%2FrDICS5erd5%2F0mPw%2FHD787xhHEGcAydQ6mb5I9kj8QunREsMnQuBE2CVjvVYr8JylIWB9kbQ8Rd63xwYoUKnG9iQmAN4ExRFgwVbj3RVJ4vUXrGaF0IVSXoX%2Fyn4WTuEI5fQyi0Ml%2FwbVubb%2BRukbTzFG0DkYAPaq1ITN2M8TnCm%2F8qKyYRjqVDWNvIG3s9zXTzt3bn8JvqrYys8yyASg4vdAqQJNIouUE3ZmFOAbXquupOU5gRIJb2AgoYoOhHFKe%2F5RibTgQASF1RQFIz%2BVducDAkmA0Or6kgUNnWxhpu9xX3JvdDxbRUwj1YC%2FCIECUWtcHrOZTfTq2dJOidQCmSCeRx7L%2BHmeC03C5OI3c6QRrLJJaP9gRgu0K7rMUJ4den4jvKl69FPNed%2F9KBHoG7n%2BkTfUB1ZkM1bPtMVyPfDEImNz9let3aRvQ%2FVsv7d04GDCUj4XHBjqkAdCoxUreWMg5fNCF%2B5rsDERfNLnl1m04SklLwTHMhIttlFDC44FsqjAzxsYGAi1b43uIZOU8p5vLjLrbrO%2BkTZz%2Bb%2FXlmO5bX4n1MNd6vh0jgvwk2kTFdasDhtlfMLsJiRYg6jVkQBQ8%2F3%2FC%2B%2Fae48q27GnW7zm4cVzc4RlYwgbWzQhmS3mwxrEWGBm5ngeJWwcgS5%2F30ouIKt8gcLEwQsPY6g1B&X-Amz-Signature=5db015de266d0d92d7080c1db86ebf425b728ad0a2564c9c48a6c3d7678e9a76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


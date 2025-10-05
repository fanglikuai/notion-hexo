---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WJMQ2IOH%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD9zVn%2BEGTDkRin2GaUTOn104UxYsWX4I709Kl3dA4FHwIhALrazpRqspWOxQwzDvFfcoxQ4Ug%2BuKG5hgYMVDkfWc%2BiKv8DCHsQABoMNjM3NDIzMTgzODA1IgwZqVlJLCPbiFiVy6Aq3APvoXOb7GIw5OEllY71sokq7apv%2BtOyNzx6w9YJUBnC7CjI%2BdMk4QXcjuJbQHzbDOoyN3l18lIAgG%2BZqcciLejIy22o1bqe%2BCN%2BLc7zZ41GFPQbMSDwDaVKKZU%2FJufpLCgnAyCwi9BTGAVhnC607rmzMjggKp4Iz%2Fci%2FnBUfdL8SwTy%2FxiauYRRjz6T1oaGkgqhBh9hnbMFCccUSfWJoEeL9F9YPljS3wcAWOo2mc0qLRnHRxArVmyIf5HqCZRua7pcSM9TLwrDIjh%2BMkto3Uylc1QcR44oAz8DNOvXi0DwlY6%2BeI2A%2FymNkKfU5Q8QIrm8YZ85z5jGF7Q7d%2BT%2BMJYCM6v8SLu9ShKCTEeAuCmLUSSh7STIGRwe0WJsVWQJJUT9JOlN%2FqGoY3%2FIRm51r96%2FpA5K5euSOQFrohe5rEyawBdb57dig%2Bknj5yrNOXPHhCLZJXAvpb4bq1ZckHOYR7hP6OvBzM78%2FF37z8984zzOvNVh9Cj4ugPCgJgjI2nMfILcOssNM1BQmWmJfhmNu7r%2BxKK%2Bot%2BLlsBtmVAOewsApvSuG%2F67D1fFzasTyubv8d6CUN0Ewgnc7ejzRs1l7LvN5yd8O3O9A%2BWSjz3P51xvR7S6rkAjYWGc%2F1zEjCM6IrHBjqkAfO0VCeGUSbQ9fSJZKZlTMJ6WH3BILAp5fCfN6axASCUg2ofPLrxKBEDwkYqnbIsiDUII8R4MFmpEO5KFk4%2B%2FiVhEtlhAJdheZo1zv10SWNrZ77Bwb3frmFCLo6IMZsbFcFZIJU%2BzQv4x3Jb6e32Q1VxFIXKvfL1eTlQVhilVd4cGJIDWSGSSm8UxTh0lkrkzmmkltw5uD%2FnweaO5T3d9A0IGVzB&X-Amz-Signature=464fb2fdbcb7ede9b9bc1418b6257e64fb87fa942a66b3851735645949fe85a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


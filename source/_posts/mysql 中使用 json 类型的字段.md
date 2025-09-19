---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XMJ6A5T2%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T230049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJIMEYCIQC8w6c1iGEyG7X21cF9YBgQqt32iclBUgpyCVmHFWdgkgIhAMaz%2FXG5e4sSORNRMXJxRe5zvP0FuVR2md%2F1yyqzJNbpKogECOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxaj1SScgkkj5qAMYYq3AN%2BniraGo%2F%2BlzQGaW9%2B%2BmT96U3yTJxhklGJ5X261QJUcEzdmbu1FDoewEL92CdJK4yYuP%2BgYBbwGu%2F1lE%2FXWh%2BPHoi1fwL4%2FJdXa7gUVUDMqcV6VZvW1W9N1XDAHPmawCpiXn4A9z09AsbwmuCqyRks175xwG48u11n%2FDfCGqhk2ToDO%2FXE6oIyLTG7RvOm8rGjhRhWenOj0vtK1nqg06BfTvoSLdWdQg7H51rFAzFyyOmunK6873PC0tI5U1JvSuNhpKfstv6ncNHCH5%2BWpsStAM9PWW1wK1c0oAo1pEsgYYEVOIEBqHk2eB3%2FjHEhPHmdXKf7b6mvS3HZZ36oekVabrwPmE8tyl1BPiKI7S7e72bcBKkM3W78%2FGv8EtHJtJL2jQ6fTJzHeNjZs4tuYl0PeyDW4H%2BK62uUvxUUYIm0RuZ8icbVa9EQjZc%2BpfIz8fz%2B6p9jDCILGVLwCMyRgP3oGSqaUmDpui06enIIvoYKbKNINVb%2FauIY8Z3SMbg7aEaxxyGMCIzlPgU1dK%2BDIbm4WzQyfSD1EihhoDhYbYYKAxXBVuyfcV05kP4uWoR%2BZwY7vN4czbWCeMq%2BPC9YGZcHAE8lysi05%2FNy1EmRbEJD51fJKYgWQIgIxfCxPDDNr7fGBjqkAXrf5DmHsk7XoI49WMb8WaXWqECpXxKzFQwZ6MK6xIJncfZcpStdPWIzKLElb9hWtuS75%2BgvNrLvK03RJoS9Gum8J9pGLzzvxdopJYrKb9DwJMntYRg%2BJOJq235hOqVTmTvGfW%2FVBYRV1BnAMk05IbSU2cs3r7h%2BG9KR47pXyfAq4lGnbleCUXSo2fkoZmtV%2B6hGjPl5%2FnKElL7cg5v%2BxT3KXus1&X-Amz-Signature=b5970ab29d9e3573e2cea6a8adf5f48d45ffbe6f0942b806aded7abad80528fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:55:00'
index_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
banner_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
---

5.7 之后支持了 json 格式


但是在实际应用中好像不怎样


# 配置&使用流程

> springboot+mybatisplus+mysql5.7

## 代码配置


java：


![imagescce2478e5401f24de6234fcc9a70b5b4.png](/images/476a1133e7aaa3e257f0f6fe9cb407b6.png)


mysql 中的表：


![imagese0bbc4d10d8ec7819433a5e83f307a52.png](/images/e2532123fe03eee4705d5db2c2ecc85d.png)


## 配置类型转换插件


```java
package org.example.studyboot.demos.web;

import com.alibaba.fastjson2.JSONObject;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

@MappedTypes(JSONObject.class)
@MappedJdbcTypes(JdbcType.VARCHAR)
public class JsonHandler extends BaseTypeHandler<JSONObject> {

    /**
     * 设置非空参数
     *
     * @param ps
     * @param i
     * @param parameter
     * @param jdbcType
     * @throws SQLException
     */
    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, JSONObject parameter, JdbcType jdbcType) throws SQLException {
        ps.setString(i, String.valueOf(parameter.toJSONString()));
    }

    /**
     * 根据列名，获取可以为空的结果
     *
     * @param rs
     * @param columnName
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, String columnName) throws SQLException {
        String sqlJson = rs.getString(columnName);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * 根据列索引，获取可以为空的结果
     *
     * @param rs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        String sqlJson = rs.getString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * @param cs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        String sqlJson = cs.getNString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }
}
```


在yaml 中配置：


![images944ad29a7fcf96a0c51a577d6bc43317.png](/images/4d25cc1863ee3e3fa6ae7e6d4c2a6cf7.png)


xml中配置：


![imagesd6de49b9a7b17849e0d393569b93bca5.png](/images/1067c14ea63fdd81764edc7b0b6e9828.png)


# 对比MongoDb


假设有以下数据


```json
{
  "name": "John",
  "age": 25,
  "address": {
    "street": "123 Main St",
    "city": "New York"
  }
}
```


使用嵌套查询即可


```bash
db.persons.find({"address.city": "New York"})
```


可以看到，直接被秒杀了


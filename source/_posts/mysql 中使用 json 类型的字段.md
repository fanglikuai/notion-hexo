---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHNLD5MG%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCDf9DXVXm0Nis2IM%2FU%2BPwanCOJ8b1iOPoMWXv%2Fbpz5xAIga2QizYqBuf5XyZE%2FeVLsPIOPTorr%2FfZS36Ql%2BIS68ukqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCQYouPbjm21SP0wAircA388%2FSSLxTyhyv4aLrnwnCW9QL1BaTBY1W%2F%2F4ZVZ8OBEb17yO%2FJzCk7zrmNka5XRacht9YlHNdm2RrfPbYw7YwjlsSFZ0%2FzfUv1agQK0rUmf42mseyhoygS4gdgsnOGLn56mqYTfRSV3zrPLg%2BGlyCclKgjP2RSGRfrz5AYIkvti0qvfmGAo5oMyhuhTmnXfZDO7Cu4eefE4ftg1olWhOmXF0V%2FMuKgtjdp0O9WqYRk%2Bdfx1hqDzXZfnTN9Jw9Ra4KvAZmGBl9re%2FZT2ZhgBC2hwE8ZgvSGu5kD8QxWlKLxKOb1SDCIPMF%2BM5%2Br20Rl2efaab1c18ufuf3HvI1bI7%2Bi3tSIT%2FCWnk68AGZHkHRUS7S2JSeH7SQpM9JLUDFv64Ir3OczU%2BcKB1FbCLPy4hiCiF0ty5rPBCmLF%2FwyHxdK1g5RXjh2hpSRkMHpLua2TQlJqbhM11IrIfhANvqW4U7ZOalSLQZLBEgWqt28bvzZRGypY7TAPOS49ofVuHm0pEAb8z%2FvwlzMx1RL4AVk7X8fW6kPGdY8x9eEzbw2jfpspMUr4ImbhL0YpRsFuiP5ynXxpmvGZ%2BJJ5xFxRvS2%2BSwnIoWpKavLvwpNdOwGUQaZ1UDjFeMwiNEA830hmMP6k8cgGOqUBnm0Ku87FWNDhK3CGZ3DpJxVxLqtk4zRueKziQOJWy%2FbeN265P9IOES8iTbCEkovH0426EUwFiAImfnI0UNDpiScBormu4ZnLzB6CFrotSx2upCN0LV%2FXdrqPHhHaBn%2BJY9DtvQl1Wyj3hoSDaNf%2FM2kS79LTdULd5rnU0J7sGQRH%2FPjQ6c3zq5%2BgQ%2FCXXqWYvhMuq4Ibva6IcytzuEebnHLC8oyy&X-Amz-Signature=162fcb2b36351fa2a436e2397b528bd379c8009bc0da38e24f82fdb42632509f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYFKQUZZ%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIATOLgUMIExknlduCGzVp2sfbLncktcapfkza8d97CrSAiEAs%2BhHTlcUwgAAchdFDLoT8cDJ7TgTUhFcEshuNWtnbZwqiAQIsf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJaC8FjoMMSYR9dDQCrcA70wNTNnrIMCk2nHHMiYoDLyKQ9JfJnIz%2BmzwQzg6M%2BkjEZDGlrgVH%2B8Y9xG6FJHEBK75h4WJZVWtTCL1V3TV1Dz8ItMhcsLl8ooyCVRRgSWu86PDiVa7t1QOR8UYHGhfcOJ0fyUj72FdzNNP0Hyy7cnTJfRHfh4iM4VmrELlDXK8YrWckg3yCbqkEoWWXGqJjABpdAAlBFnZtQTsy02xYkSMPw%2BJlY5gtj3IAYXzCCJ8mYxNuQnHmob2H9wcpuQrviMRuc%2F3d4PPAQilGjrJUoJ3UgnzcntW3FctmmRCaLW6PVz9RAjvKqhZ9TnD5t7fHGdO2z4Ph8Txzf3r4Uvy646ZM7wwOnKV0boBfn0bLTPA2vvpKbAuERhybrhRyyIxQM9fvn0xZHFch4qTaoEWo6BI3tY5jgyaODGVXAJoLF%2B0Y1M0vA8gGnXKrMNNxsAD89YAUMaK04kVTSQLq8xvX0slz9wBVr6qF7caGXWboanIT2naK%2FduEB432pUIkyIJdaO5%2F679iDB5Pf8Dtou6%2Bi1I88eW31JS3Q1gsfmDAuxcel%2FNB%2B9vssszvtYbtsRcRaVwfgbaGDTLNQ%2FTfSy60aUIhYKZyIIvb2h858KBhzptH7jzP7QpcGdPvzSMKyZrcYGOqUBt1wqL%2Fj4V1A%2Fh%2BrTz0yzdz9mqPWtY1ajGpY5unJMKIg3LlxYuTVKHFi60O9JQxhcudtyy0IYz5Jfdjf0LE%2BhZEvO5B8FPYjojzt0is5hFZG4UD%2FLTVN5DYB6klT%2BWqOM1ZsxohPH3MeqlN25YZBqIyGL6ec67Mo%2FAcablQ5h2Ndve8QXVWPUyqoAldP0J8UGj6%2FcObmCgTJ4NlWOZq8oInBgOP%2F9&X-Amz-Signature=8abdc087cae56952dbce31c2961210d001fa90b0ac3d39711962e0f778b4a385&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


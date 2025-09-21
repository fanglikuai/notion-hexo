---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R3FMGM7P%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD73ZI3J1Y2%2FknmsYKNexjSgUOpX%2BprI8uH2Kg%2FvzPgkgIgdf3ykqU8B1BnufPooT6JIDJ%2BC4MRqPwbisM8d0mfVKsqiAQI%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFEvwY3a2%2B4S0YYWFSrcA3wTqsDcednZoesHeqr%2FZcUw0jJ%2Fxz7%2FHgaikkbIsNta98k1TzyxVWntoby90webOObTJ3fmT2YyxywKsP4CA2%2Bsd40hWHFvWnF%2FZskHohaXOL3zzNMqvHqJk8zURYp4FTU%2FHh9XAAzdjmlcj7cZp0jBdWPZO%2FHYWpyVxRWA8LVowkqrHq56klLoVAIye0VICN23%2FZuFFmJpONs7LkVT6rdLjDkkwgiHGUJSlQ2awX1I48RTapXzHW5dT23W%2BLo2uvBmD5F6S7rC4YnpWA6Z59mv5YoUSXG8igsYoFFScobrd8n6FNzpAOcQby4G0xFb%2F6qurcj3g4a7iXOuhoIyFgSkwLXIQ7VDbh0tMJcGspTHRviHdL5rBNjXlDqjpcT%2B4GGY5iBZqCzdGN5tMH8xfwdTZlPss6EzG6CwKgyAsn0L%2BE9%2Bc8IZw0YzrqBAdfhbwe8rKKyMybIoaWG8hWDVXgngXRs1rW%2FgTFDhlD%2BFW1%2BOh5ggEj%2Fm0ihSVVYi8nCvcN2zRN7I4FiJzDiCtw8UH734LDL0Je8DO4mhvTH2ykCc%2B1JZfp35gqzlxGgn8fXKHNtyXFTgQjNtswsWHUGbKrOxFeLQUDNDsRwsD0X4aV9Ln%2Bzz1AoN7AO57W8qMPK8vcYGOqUBDT%2Bbwfy5m977AViP9DFJ3APCR8tMHmXn%2BQQIxPdYf5q24W9hko44uob0FYXrlWcFtYuJJx%2B7hOgCjIrn0cYU6RXD%2F8oWV42mjCEo1VEaegxiNdiNuIYtOXPdodX9BUVeSt33Y6gV3kOa1lByPJh7jf8M0oODnjcQGX%2B1S0ClzT%2BHlpRfi34p9NlXET0SMd1Z8xioSOd8jMcxtTHW%2FBkGv%2FBtkhsR&X-Amz-Signature=f2d9a386c123179acb4287e1767d06f0420f7aebe6bcbb035fd6f990e7b56e99&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


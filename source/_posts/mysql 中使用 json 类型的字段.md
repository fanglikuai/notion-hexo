---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XVGT5AQD%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcZDoAkkDi8YifUHBxCNMYA%2BjkT8YBSdQtAzIOUVOXNgIgJmmWi%2BsYCsa3Mle5u0y1UyTGQIY5QNkKLT1h3imM1g8q%2FwMIfBAAGgw2Mzc0MjMxODM4MDUiDE2%2BFL%2FQXxSB5UB6gyrcAwqDObrFnePvKqHiY96b2MC865udqCl3ldDmyImdhEpcirGgEfuBIqi5QAxjqnROWkohiM4DvfjLoKlbm0%2F%2BkQp9D1LuDkQKi7H9ORiaeRDhL5U3xAtjCAHgjVWY9vxKnSJVXYey0FCDYo194kO95jlFN1hjCoF8%2F0J%2B3CENhAWTMjjJPw6gls9HhNTS5g9ndIxeORsdNqR6rwOfW9DRGvRuYmvE5264QJEhIsK%2BC%2B5ELLWPtV5U76JR5dqnaGFWhPf1jJsxXsXGqTJ6GP2mfkVIXyq2TBBjZy4jqKpsHNeA5nfVJ2RY5%2FvefqweXjHxhY%2FDvsikjOkm%2B5x7iNMDiTu3WyWBXqa05LEfqHKpTuN3RnDdABJU3FDV95M3kQ33u%2Bfcg1R4%2BUbmQpGg%2FeSAH3Ht1A%2F92nND%2FnniLMgdGpHY3B5ehnmqdyECWNrVEdTc0tnMZXtb%2BU6zQbF%2BkFaL14fVMvOGla4xnNghgEAJV%2FUF%2BtANMzKAWL6ZBedlnsYEgzxEx1nL4Tr19l%2FL1x55%2F2u0fzYwKxfsDANsCJ1P6nRQtvQNKPc9g2tnmM9XsaVv8eKaxXZaUC5g9eH%2BAr5L3EeEGVp3KBcFOxDued2oVSWuAMfz5ZjJkK2HUWswMJqiqcgGOqUB3C5nfJiyXTuWZI9ndUuK42vlKbiWQ%2Beh%2F9ls8yaH1pN3feyIYmgzOKwO%2BS2F%2FrSsD%2BakELk0CKnXS315Gm8b%2BFt9ZS7OT%2BHJlXCYJgtZTXXvbZ0r%2B5caIG6m740YKB9l3QK6ftKskqn2Btu%2FqgLrBSxqGtnfRPezy1p4zQpUzjIMvbFGmNeEByC378HzcqlIR1bNS6JArpBULzJ7UZTmobplkTgF&X-Amz-Signature=54bceb6feb5a4c0ded299eebffa6fbe666f539d2736f41670e128e11a56bb39d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


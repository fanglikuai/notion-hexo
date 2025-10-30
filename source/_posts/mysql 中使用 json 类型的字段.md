---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V7HH6LI2%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQC%2Fh%2F9d2ElSR2Duhg1JbxzzAdEqJ04A8WHf97m68a5EaAIgKwiOLIx8h8hery8bM%2FteY2NPpQ1PCdlx9IpjrGBV8aUqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN1cqOGa3MofZyLDWSrcAwiqWcnZWQB%2FnssKhb0cHQUBVrp0a4I4mIHHzTZDbbUQrRnc7%2FRodQfixEv68s5qssFYdJOU7K8DyjsoESrqkWnCMdz2fYgAMxQJVscsGBbqOuUxm%2FhxAbHESJ1JLDrFuwtS%2FEns%2FdthObmtvh2RWzVSZuSzYcx0cSqf%2BjeWdZJFf4eBoD8ndhhbMHyJBlGPAlaxlkrrepcy4wgnOxb%2Bwllt3laMZsd3Z%2BuZUZZ%2FXNSKXIe8zLUN6guvgasxmrprMcax2KDjiKGxSVu5Y9f%2FO3tS6MzYKzwBMWMLchcMNIPAaiuVC3yenyZTD7vVb7AzOuwOiAB9jQORJSYWoPq%2FamGvArsYIiDJfP%2B9utreZEzZXXWG24UylqjbrpV5NuAPCDk8P0uarzhinIU7sl%2Fx0U%2FLS%2BHBsl7AsNbbNaJIaa59VR1dCnv2QOFFdes0KeJSslMqvIRF6QJohorgm%2FZ%2F3VYIftHo0qOAoI6V%2B1z%2B5N2C49t7KVTE5gPzFeuKvRzEzhBeo%2FqsHF0l6VStuYVgjLkE5tCXwcRUFfI85YeYeA1qnQx6rliaPP2KAQfggAHosNkDXVXIpkcM8KQBktVzwfe9JLACnkm%2FW5BR0NKJFuiY6kabZpSWp3qU6O8fMLSbjsgGOqUBsiQbUxwpZ1G7qSv%2BkN49cDcjNWdEQFQwK2LPfYT%2FvjK9lSOWPCFJ9Ps4444LTdewXtYfyx6J4X9nidOdDxn8rLaxNPf01LMc5HRSSvw4AW82UFmYAUBBtIaE8rooRXKmHhcub85mQmqTSqqiBz%2ByWmkmPIIW3fTr7Z9d0t1DopoGuFzb8eNiNjmXv9IXeltWrWf8UVwvloKtZU%2FT9x8iFsLKYaJk&X-Amz-Signature=a1f7f2341f12a91a4882006c408b55eedaa6fbb7d8f268d9ad5972d048983b3b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


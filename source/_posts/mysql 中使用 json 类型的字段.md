---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JKODN25%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T100043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBn%2FYEqPVTNScM7nL1Oty0BfmdxQxEzLQd47MTrsq2HzAiEA44GcTeP79QmkOBBHdFrxVL4yZfmXssNXvKJ1Ni51W74q%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDGUJlM13MfwfoDzU1SrcA3rVo1CedfQlUh%2BKgGH9VkFXx32xWt7T0p72BB1k5wUgiwHuVsLmYgl3PPYgmyy77vH6%2F7O6IrNGbz8KAmAk03tGWTxxFnCAFY7oe%2FIeEYK3LDEuzTHb425FauDKIzqvHlDS2iv4n8e9y2w7C35XumMa8%2FnjKEsw5Dxf%2BQi5GwSKR6s9KOJkOqvlxHErf30HXPJzRvGtyU1xjNtC3LXvx%2BAmChyjoOofar5DaYFnrTad1k7HcMt3qDThIasMy803G%2FM%2FnYOl8G6cyp%2BTObQsD32wk30sNteT4Iajabr2cBHqgQqnLz%2BtlLWBYWj21EyNX6hDCAtT94nvuh3ikDliam2%2BD%2FAFJUzGtqk%2FO5ghLUG4fkc%2FNh%2B5m49QdTY99rmtDQrHeqMQe%2BwSLYgp0%2F5ojQwPfTVfXpwmDhnqm8qaAZDRR8j7m%2FO9i2neJEzrLLC9iHq3yivWX4mYM%2FvvZDnwKdaNvqyVRUIUh1haUy0nTWrVb8HsrRI1uZWO9XSJU0tDgezm5q%2FGseON%2BlWSMa2zg3w3H4oG3tfK4wMjf8XSDGOYV30v39aLh2hJMCOSxQCnKVHu36fzZyeIn7Hr9mHd3NQTxWB3twR9%2Fwcb2nU%2Faj5eJgA07VMcTMAP6SehMLb5zsYGOqUBMeK%2B5w3O%2BEdhM5ZI1IlWTStYlSxx0npxZ3dQepGZpggicS2K3H7hea8Myi4xFGxxIfMr3QoaVKtzKWUy3YriK0kDEZXeganBLpQzj3Nv25E9o2KxlhlMdptBKSa1OCtwy8vNyh9zHIRyqAX1h3a3Q%2F%2BxSZ9FB268hjfkI%2Fn6TXLVRKGuki8MUuYbVBPyVlKpyJSrdYVp%2B992Ma5pqvKDRvbJpfh7&X-Amz-Signature=fa490123707b513c8f65704ceccf23313f001f4bdd0a5067036dc8a4af040ce4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


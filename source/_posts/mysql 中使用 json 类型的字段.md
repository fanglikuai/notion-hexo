---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WYFTRCTK%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T160102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIAQyVZPJdXive9E3oQRpJtsO65k3NLdFX7GR8avdDio%2BAiB6kyW8imm979xNM%2B%2FcqXg2tU%2FjxIeCI8DC%2Bbn9pQMHUyqIBAjZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMkuUeTPR11t5PzSTeKtwDXi8snyu76e1N2o0OF5V%2F%2Fjdt6Y%2FAmIfBdoBv5kBXmbftqdyo9xSyD%2BG2G3n5QoIKeWp%2FLY5HiD2e2t7E8hdCtNPnMEYEohABjIl4HVVoQyI8tJSmr%2BSjoGQEgpdjs0Elr52Ba1hN4tM2gfPE%2F8r3WTPngldFEuC2KznA3q3kJIxJk1WZJHJS2wsIf1y9SEbyxnZrUrCzU87sNAcgS%2FzyZw4jya1pBCp4Ok%2BU84OzjhgnapJLaJKDSfeRGudjK0nAU%2B82S1S%2B0jhOyJ97Z0ZBRQAJcLwky7jf2iu2B6xmxZb%2BZxI7bzvXRsmRzKQcRXFS95eqcmtRXtHO02ft8CJGU8vXNW8ENoU8qreKlM0cDyeSfFbvsJbY7y4DuU1gMj2E3BPP35ywj9vry%2F7mHMUqsX21gOK7Q4AC1mmoQ1uN%2B40m4QfIZ4KqEeekGFdC%2FmD9jwfb5wKrDNbjO2Of0NdfZ2sSkja8lpe5VV7ymrHsBxvF2p625G9QagueteWvIgWPDJEptIN057SmeZU%2FOJp%2BDug4snawt1KkO75xO%2Fy%2BlFS54QbnR2u1c1TwD1ZiqOb8VbXWTyY6Pn%2BUKOyopSiYspfU78ylr2pc%2F6zf0keD%2BxCEAJ%2BclPmBaQk9bFMwjNXqxgY6pgHFaxatAtIzkdzls7nkq%2Bl0QSKkNqfdFefVSeF0RsarG7PIgCs20q6F2Vo%2B20EW%2FPdwNtY33LHvGcusFDhKdFduhiz8geyGC6N83%2F7DA9f%2FNdXmAYKDDaXJEexBD2uvrne8X2c0zmEqY54CGa6WIYH%2FgIT9D4hSXtxwlknr73QHq0oChwK4AXQvnMCxTtKMkME8gazwqAvDDliqDPUz4j9zgA%2BUab9n&X-Amz-Signature=fff9a119ddc21f150651bdd9ed9141d724fc834820e7e931d8e0f5b0a2cea83d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


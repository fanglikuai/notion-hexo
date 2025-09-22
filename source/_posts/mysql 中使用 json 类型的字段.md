---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCCUXM37%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T060041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBWqav415LYSKRgJyQRSrIUniPo%2FKtErS%2F5jBAMy3zlQAiBCwV1YDyyK6f1phkW%2F8l6HxQ0uzOK3gX1S1XuT00TVpSr%2FAwglEAAaDDYzNzQyMzE4MzgwNSIMpNoUvEsI46%2B4ULSUKtwDodENnDiXuPUBCvbvwtGA%2FD42IXmXk%2BSqFn0f4plWElWHcR%2BI24Ie09U7WCkhIeAbGZ%2FEy1802UpFImpdeEabk17%2BoCRZCPiIhXcjpSnzT2gcT0%2B94eVmfwnrHqE6nU0Szt6l46VwQWSidvZsaq4%2Fex15Avam%2FOyDZAbqmRQeykMK2POpLuwKrq7OHwZdzocf0RKFAOyGALckZ2vyALYcLa%2FkPRoqT1IYhzzgFYwAvEcO5bBnE8z5L0WPB6qb6b8FEwTdYhbcsP8M0rRjn7eoomF5YLZKnh7Dt2R2Z0fVzdZ%2FNMiEK1sVJvP%2Ba1JF9t%2By1TW1jzjxxho4tNJzSmwAXs1e3IdeH6%2F07Ge427dhyHiihbYCsYwA%2BVaXHvdHPmkHS644TZNmcJ6kJAD9d0IQBcNu8hZa33LtrX2XFSVv4ThzDfG35AtZCI%2B15FSTziz0%2F3yhc%2BhPWOl3SiE9%2BxxOW5ua7e16zksutT%2FXfuvnZolXAlE062BApP73i5SXPZ1ApsUy%2B0W8PVEmo9TOBLUsFHuM4T%2BHt4Oy%2B2YfYF2524fq6%2FFCLjUKN9wZF3yzG1Dm3GJ8V7S18wroVHSpciODla1bCRpIIQqlPmQjKM7CY66GXpg%2FT%2F6OioC1roww9JLDxgY6pgEZBXfye2g8tYlMcSIRU7GP2ik7vIsdnpnvgUGxc9NnR4%2FOWNINYPaV11MLaswv7k1WDKmC7RwwQ%2FM2ShQHm4Kc0q6ZxxwCweO3%2BIfYXNCtDwu8ahXiXnuCnpSTUS1TbpOnS2HtTnzJMkDMM0pCvnRpn1CJ1niSUBUnOJQTAax0X6O8Jv3Pbskm48lBmJapWJotx4sFtppBXwQ0PG0qJRM7aMg3wYV8&X-Amz-Signature=dbd6e92cea1f8ee053cbc20002f661914ef3f8abd1f3928a96202a64b264dfa0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


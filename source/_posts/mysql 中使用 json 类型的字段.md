---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YUQX6LS2%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T080039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDvK%2FQJkdkyRnjCmalLU2UXwgnmw4DZvUGxAJ3%2Bxwc44wIgdYOanPIxUppgoue13dPK%2BLn8TKX22KYse0GrhcM%2B91Qq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDNHTfIAK2tORVpHDHircA4laScqPOVFbjgc5oDjYfgngTwQ0%2BeIUMJ%2BdA%2BZUCTunNndDoyzg2FLcDLyDNLUT%2BSvxRS943KP46Fk0uZ2cThKKbF%2FHV7yhTZxAjgxjzGqwx5QgFkuPCEKbc3ZJuYC4edxEeyj%2FJsEQEO3mR%2BeABnDu%2FSldDC5Eg%2F6XeW4sqqFlNF9DK3jCKB4uVsPa4PA5u87J4pVkW3jqJ0StiVxLsA9tH1egj6OAG52MXx%2FpS84B5liKqQVJ7JfYuQ0%2BFBFvp2ysIytjrnW7%2FHMZJRvjdG0wUdRkzdojv34FbMsbByuUsR5xv64Ys3DjG2O3DmdNYNfEaJhr6fF5lffPNc4cS6e%2BRkqfnLuA1EMwCnDnMyNdP%2FfbtFf%2FXPtZToPx7%2BX5en4AKjMlyWUtqOMViv4nIkA3rLXqbqhKH0q5dAAkhBw%2BkjdKPxnloVJgbVmESw9ahtQZ%2BQfFhzoA7Viun3WSpg5O3AMfgHxaeHQFrVVLeTv1y9rWyLm65OgMtAezPpfb7%2BAZ9%2BpFa4vYWggjj65DfSPpZFCrBkxcSI8Pbj9cccHobWMr%2FFrgrRPZmklApyCe1C5xcXb5C8yKnpinD%2BvpfUEVC5BZTys6D%2FdD9NB9wdOKLh9V41QB%2BSYbKCXtMOeAuMcGOqUBzdEdVQAhvqgNbbpN%2Fd4PR0SlN%2FdThEalR6Pe7ieWUnEd%2F3NJhg4rWsFuoBSKI5WTgeWtk5KBVIDPzjKGtmHV%2F5y8oozVdi2U52KoEW1CsIcNjU00ArQbw%2BOdfjWBi4G7dNB9L%2FlBI8NwinQQsXQIhUxmyuJso3o6m8cTUYvZCHjn9rIURJEwGbOZKPMgZ9%2BF7phN4dcAaEfdZALpYwTu8N9U0mFx&X-Amz-Signature=5e0195e4b0d45a9a98c3aab81924bad6bebcb49c52d6b3aa5fd5f018b08e21e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


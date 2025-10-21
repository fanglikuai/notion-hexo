---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622VF6CIO%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T020053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJIMEYCIQDL%2F83ZyTM3xH0ocglp9mDPKykOnL19%2BOKKcoaVe0vlvgIhAKbSCtg08kvgb%2F4oWUNSBHdydEM7l9YoR0%2BMfnecVp2xKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzBhZo6bDZpHWBLzcsq3APR1f2BhKsbogFfyluBHULkJZSLiCFrOtjHCvolKiu8hMLbdngcOZx%2F3eIpTXCWp07iDyJxZ%2BRp%2FoLOhzmY024B%2FzMiezlWRmbPxnYQq1F%2BWb9WWDB2TEvSVB2KL0Gw%2BcEitpiTICzCyq1SZMdndthB24H%2BSSsbnk4kDuQJymQ1orfn0cBFOAot2kJBF7MpxCjhY9KpSgUF9hHV8WXgV3KMnZWcBVp1nyFuDNPwiPVdIoaE92%2BV3UHgKEeZQqwYMwEo7VkeUpwS6%2FAVP5CYy52qWp78WN8oMKAcqZVoUX10UNmak%2FqV4zfc1bF6LBGJ7RBbqJoDrrnjcxtysL3MuDbTjq8q3mhQ4Rh7LxbkLJdkTKaigqXMTfg%2F2%2BS%2BkY7XOuUdFRH3XQweoIvM2tZ4SvyNwVnFmpxK4RPrtx3tulVOPBMd640E75Nun4cjMH0MVZtmb2xStBRFQ9sz%2By0HO3zdx0ng9%2FepBNuBR8qcfhJA9YLN9FsdPnQWpgr4CTg6f71wdEs3KAJsKjhKo%2FxYYVZil2KqN2lxFpcqpGnjEUcDpKKLribiXoNYWOaMMTEhJgUd27jZ%2FwOZAfn2t1oh2FoRqP0ks%2BmINLT1wQidB74wXbQidVtqyESeFGQaajCnutvHBjqkAbR7N%2FwsICLmFkn3jHmfnA4%2Fu1fxrqrLY8ePrHkCm5ztRNiVca3LW0vpKygHVVpchOD4oSE22GyIs4lq%2FBsWOgJFv1UI8jrroIAohWK6Q%2BQoFZzQEYkSORc%2FNFNHNNK%2FdL40DhaXaCVW3sZF%2BJnPDQYj0NBoc9D64cOgza3TToUM3izW%2F9ugRpQ%2FzzzvOGXr0B%2ByxnllEDhpqc%2B%2BUyQQ7c9KLim5&X-Amz-Signature=528eb76e161a86e20834716c226fe631af60fb66daa0cf8706cca1649197e05f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


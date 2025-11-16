---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W6LEGFTD%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvUJhnbLvk3BjeHrzPf59%2FC25wEnT4zaOwCRR5NgLyQAIgQeWk3Fzv%2FvJxaJKSrVbChwagUOQthLhZqcurDChPTkYqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKROUxgH1YdTRX0t5CrcA%2FqTeYg59YOoZwwHzFtk8w213IzBOYfC6cIZNgRc1rb2mkOUHpuD%2BPEWhtHnp4lCN1ap6vbN4no4fkQbxqapELcY7Cvm2FvKQacETVUwOtZuPwHrYIXXDSm996DpQnjcH5V9u7sQsg9EjfimWaYNfD6iNJ4R0vPrI59VYxS6By8WH5eikrxI6xPay3OH0obaJCCrEIF11OGBiyYuxBZzQl%2FH62egfy0u7qHyVQcx7DqVMDN9YZMNM9gsTauDptHXgtu%2FpLCRr5u0YPW%2FQQuFOz8g8seHghua0XeP7zltyd9%2FZHOaYWXqs%2F8OSvrT%2FTv9zKtg9CTXVckUkxN5xGuIp7czrcA2meEVEhl8RuOGJu0di%2Fwp4mIpDwxwtnVPPN97lZKhrqLZq3lfZ3ThaSbM%2B0Pa%2FXTCeTq6pEzJ6jrnvFTPZgfF8afMxEJqeDNEKoXPL0ddmytDKEWR%2BdSTAUt3Al7zogknVZAXGHxxfj0VJS7c3DPtDMy8Vjpxto%2FyeYAqIhmHvyox4Sn7KB%2FOAwQ12D9lJRMV%2F3NEFSXn21nsFWNDK3%2FOfom%2BXBFjLFAknxzTjwius6mlisSPI%2B6X1H2fY6wTm%2Bn3RVBpVlJvSXjzKYJG8savqobtwdkaXo6QMP795cgGOqUBQFfdXt1%2FnPj7yzZqbzHu9tJ4CEuj4PaYYBxHiZjhMuZKoABbPeZUQeUdf7SL1xurza1lTa4BEux03HSx0M2IrNMX2dMoCHLjLMQipUGDTZfrxtx%2F2YEj8FFZ419jqUzeeZfFgWE5oEIAZpUYcJxbXfL5raBV1VhAL2u88rqgtbJgm9ruMT523t7NKh4EOBYMpjP17nt4PggJKd6%2BCRfTLOKganiL&X-Amz-Signature=0fd990226f33d3299a6df29d9a201493323934f4c25cd23147b0309602b8cb19&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


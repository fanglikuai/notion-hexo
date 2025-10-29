---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YTA2BVH3%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T090040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJGMEQCID2E%2BPGBp4Ltu0upM6Dc4%2BFTPZY4njMITtaYBtjh7VbmAiAh3wPG0UcJpIelp0YAiLGxT2z57s2YUUzg%2B%2BNpZKRn2SqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCbayZkj0RbWWOX3CKtwDzLSZ2KjmKZrwgTbZjTNUB%2Fts1eWuEx%2F%2BWg9HP%2FWIF4Hf%2FnCOdP3KBr3coUe867zdM7v5mVTATNwYgdEs1uli2IxL9vc46zQYNcHLn4%2BOVonru06OkibYH0qGANKn5MRBCXGwx4BnQlh96zL8kiW45bFbh1uzsbfIvt%2FvkEPqN%2B4A2Os%2B1Au6PlbodvdTVJmsGN3sVV5JlcJrZkJea0yI9LvyaJtSyYaKCMElswcRwi1L%2F6C4ATsb8s7mHEkQQ6posgqiS%2Fve%2Fb1zLnVA828moLKHzI2qU%2FiHUSEbNUwaCoH15GbPc3r7GX1qoNTXfvgDGhK8cqaELh3DQs2My74wBAKgE2Ss%2BVo8aR%2F3o%2FliRpfJIYhzPPOoYHjNBCgWI0RXFu1FNQ8VoYUjV7wXiiB0oDrqF55m6pEe0zR6%2BeTeoW2t16jl%2FpR0csGxjkESwWelU%2Bqgwv3Q7VimjtljSAo1eE3%2BDEzo5zqXaBEgLuwFPfflC0Y4Kl6T4f15l8iiYO2c%2BXAyYEckYU%2FlPHc771uuzV%2BQoGyw6yQh%2FJ65fqAsgcuPWqDn%2FPU7gNcPBYF4P3SEWPT0dGs6TxnxlAhzn4ZDWptwT1Mj5i7%2BssXlJ0vU78jfwC9js5AzOcl7ke4wvqyHyAY6pgHDNltgqBRLwe3r58n93ssyHrUf70vHkW6DighKtVzO%2B9Qwx%2FoRU9OjzV3Lp8wUviLHRyOOzOzp7I38pWBFK7bmSREJc4%2FvedsHF0l3%2FWg1O1nEdJQygzv4IfVKJ81WidGLIVEItHl7q8U9RbnxkR9YBbFlwzmiK2dIIuz4V5IMak36kUC5hEknX3%2FrZeHKue86bXTHrICluUSCUmDlq7szQ1ARIxXa&X-Amz-Signature=012a682ae5530d993f71a9631e2e2abf058df88e1464829595616aa5bd6576ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


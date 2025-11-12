---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJZYPGY6%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T150100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJIMEYCIQCZrJ0fWNr72%2BUN1Ux3fXqC%2Bj1ibgZ6YNIcwuPuuraK9gIhANGG04RbPkwfLy6%2BNFVlKJwp53plpTyYoVJeMXd8pEdtKv8DCDgQABoMNjM3NDIzMTgzODA1IgxYw79WaUD1Vo7gQEMq3AObOHp1GK9yKQMdRiRa8qiFJlSi0dNZDX197xtCeZKa1eBBsjQiQ5BxaFT7Owu6j%2FFNbvjT7Aer0o2A9A9VTAzy6zZv7sojVw2GzA7enRDZSlCBpSis19Y0AHqXGZe5uzqTM2xsnaW4%2FeIY%2BEdqiU2AL9hFMIfwp3VqNh108v80Qf6hAqkJJOhF%2FoKK0BRg6rAcuAtWyZmS46g4r93kYQ5AOcxp%2BRSKB7VZFpaxCLMYkvxQLlYji2Rtb0UErW9BgGRpnWK2f5UtkwvcyFr1QiZPQlqtY%2F2iNLxkILdHOcP83%2BrI%2BtrNWzum5CDVoAW1sELBg3m8On8g9OruwYwGI6mCCg7hWSEURn1MqaT%2BuONtMqGG%2Bv6v5z0h9LVLl6D35k%2FNdeEkCA2QRRuVUmzqj9UEtaA%2BYE2u5gyePhsNY1wgeZo1MLMbE9xgLCq840DuVUV6xqiSqbhmZdjbZDIsX8Jyp5ndYmznVBqBES6f7cwPuHm%2BdzOpn04cjCO765HEFqhTELfqcWo%2BIjCKbtNyaQ%2BX0aBl5vCbXVZj%2F6P1MVnJDOqMFA4N5H3jOH25EYGZV7ubxg5cGQH5w83LxSEdbZfRunF%2BEezpfhyFweFwdI%2BgpiEB9PVaKJCINOFSMDDfudLIBjqkAQ%2FGd8oJ%2FJ6hi3EYOTw7ULamRcgRxTT7jxz8hq8vaQRA%2B6Ov8UADVC9wteP06llxrLfuj44bwaLY8wSZVgKLiM7cjQSck82f5K2zoHHvym61tQVwru7ta3SOrADv3nE2%2FzxfNiUPPcss3Z%2FsGFNh4hFtLhhQA4Fg4mXL6AKsmwkUy4LkpPKhjIwcKWvKve6VlhPiXso9L2ojxh2liCn3nrHl82xm&X-Amz-Signature=2af817aec4165c1d78ff6c3c7d55dab0c3f27815e96e2589bdbbfe80f3cca341&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


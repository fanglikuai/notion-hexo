---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46643MMB2HT%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD6QCeNfP7RS1aV7b0NBRZQyjNW7BSeHj56uhf8if9HQwIhAMy86iL6bLnsgXmPiNuvbCkjTPsXrmhSAY0KOaeESlgeKv8DCHMQABoMNjM3NDIzMTgzODA1IgzAS4SXPJ1Jx4Yy56wq3AOMQZcsfrqmht%2Fgqalr33io8SDVXP0GiRaeGQtxYmw5bcrw3jgrTtEOJlrsYZHbSZoyVnFtoNrgg3qOmI7JnV2XGDyJt8dnbWS3zxbZAvURnpOPSMYwr73IwtebWMsH05vwureEvInTO3vHtmOVWM3ah7i6sjZsiDRWt0LNEHiE7bbwyECiLLXy3QeD09IpipH32Efkz9QjNr6SMGx9oAKHCFD%2FqlPsitKuu8XcdnksoCVbiGv4uZVOc03%2BlojQrhszq58OpAo9VDXg1X%2FnGGj9MtsMKRqvmMP%2BE57lVW%2Biipui6ts%2BjOhVaBF1p9UR06BHmXrmV4Gxf62n75a%2F7GIVdRC3F4K61KF%2F6thKXSQbgp%2FjsHW5Y8WsiAvdABSECh5NaV2D7FnNRP9D2JngOhZiI10imNxHgA7pwhXGAwujlBSCocRxladbwtSniWOzdxfMpcI3cVoJoj9p7sdzTPyxPg3j%2B9dcjccFfjv%2FHi2DgLHlVvq%2FaE0Vs%2B6jlA%2B1bRWfDqY4HP07%2F1KKeylG74Nj9YAg%2FB3%2FQIfYN5L76LPPi3d%2F5np%2BbyKJOUi%2B1x02NslY96J9dbCSZKt7ZEP6pg5gRgSl1V95BLwBUvREPEH%2FF6m%2B8G%2FHcAToxn8D%2FTCu1pfJBjqkAeoxTPd0JaohAB1Nxjj8Wp6jXEpEZg15hEsWNHshgW2hDgbcuG%2BCA2AfFiBBZWTHcKbEsaKcAJOZRtKe%2FI6nxiQIs8K93BKxfSY84VxceCARA2bIDvXtAy3KCfm88IFzJdLKDhlVAweHNJ6vhrBTGmfFFyfD9rEyPe7pmyEfwuABnRlBlYgqGHDgcW7kiQHWNkweoDqY6dvDpHSy8FiMLl%2FtfZp9&X-Amz-Signature=2e6f2ba78d49c5d35c5b37cb870f03e0f6330b176c6becb2f1cba29ed15616a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


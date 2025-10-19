---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RP4MWKBY%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJIMEYCIQDBNDRE9y%2Ff9mX%2BtDH7bBIYsgsh05jT%2BtRcd59IhmiC5gIhAMxlJXe5eDCXQ7omP2o8yik3EmDeF6LVc%2F%2Fm%2F8ygkS3sKogECNv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw0cqQB7s39DuYO%2Fewq3AN4Y6VWssViKrKHv2daAfo%2BHCQ8DwOuhWRj2ubsTFEVk0nhQjnQfKp24MydGwvhHxwV11RucNSKHfesMyt%2BRUR%2FFRARBjBqjVfCSVnmkYNK4FdStv1AwBFdC4ISuurPC2Y%2Bz2Ire2NEr5hgQh8NY1tJZP9X1Vvb8sH7oPHZcDVsvLYehBqtdAAGuOLItr1j%2BzVSJ7BCJ7fBg9V2apVzM5SKXhZz5lICClMwHlSWCtIY8TVLzkenpLP1CWwKSFIXeHT7%2Fk2n31dBB5Mpx4zkeYnSXvE5QyUZtx1snwW86ajsDpkyu%2F%2FdL2Zxx1f%2BeSL6sR6vyiF5UQwDoCicvblxENPE5KpYiGg1kil6kJ5iWxKN6SQ%2F12gHo%2F%2B%2Fp2Mjdhzd9GRTGQqJgQMwoZtnzrlu8hfxXJpoK2CBnNOeCtVV0KKSEKlDQvy%2F8Xya27FbRCNyRtFXst%2FXPaK%2FNcSwlIflD4uGrPnNb1yU2T4x7PKwbvyljQORdNwfIw19V5AW6Xlq3XKtf%2BrmfDyT4ihlUDkv5kdfWyQrAD%2FQE522V1IorkjHuqkNIdPj%2FOctNbtmO0Dc9lL1AnsxCpHSjZ27OAX%2Bz4pz%2BWzmGs89bFk9i4Qr89k4WpT5LGGM%2FEyaYzJJkTDK19THBjqkAZpb29n2zdYJJDAao%2FA1VVN6wikV6rN4RG5dJDnqOV7y45W2iB0WXgiiui0iuSCHLp2bOAKAwVAQIwRMBUF035CsN%2BpovJWYMJgZ26TEOyjxDuOWQUZLSz3YWIx%2FIotI%2FKjBLo3KTC%2F8SjYZGnP8R3q8lgfM0IeGlmbr9p9zpihxS0f14ZLp0OECQddb63Hm0d3eSWRxTTc%2FXpMJ3L3b6Q71JAoo&X-Amz-Signature=864092ef6f98ea51a8964036bc236a2b88241ac202048cbd5ef16caa2cff7a5d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


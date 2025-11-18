---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663PKMEBNL%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJGMEQCIAHINidy08HsrH6adFgVy3NoKcj4bWYwZzkveObrMflzAiAsPlm4b0I4R7MC2POmnZyYQDBbSqq2zkhJO7NjINEFISqIBAjN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMox1LfDFcyEWStdLLKtwD%2F5T976hi9ySlwwtQoGcCr6zIOQ2yFvtzYBeObgrXgTTrfMO24YsYdpIcUPzE%2BpDNkzYiZpWEogDiDFH20fa1ry5G2TGG%2F6v4hxnQ%2Bxdz%2B%2BCwt0baNgKtMLPnaS8QhyX%2BZJcc0ukXuGsLXksJFZ%2FzLB8yPFz47SjvDw%2Fo9LhsefA2J7N4Z62HJ49jY3Cvu21kU5cGOVVR%2BcvWIZ1LqfnvrcEJsadBXBp8TnXKkwHDSooqn%2BGJeifUc%2BaUQkDvSu2nKN0WLjGI14xDR8XfQjD39bxQf%2Bg0oohiUOP0IDI9e6Blt4VIvSw%2BZK471%2BFyoxGs4VzZB63mOqg%2BET7YkReuNPRVjb7VorFx7LGVY2utfX9e0ek0c8uK3cdzPCBAHzSWNrSp1yyx4oo9%2FhM2pMUZj7vgoIx%2Fn6UvrjmNYhNmXPSHuon9oP%2BPpko5DwLPPHz0eeEB%2BycKlut%2F%2B1D8W9MeR6jZ3JgGyoslrnasmdAQnQ2iGtkTNy%2B21TxsKHrI4rPt30j%2BxeRpnVYRdCpmIoYgnnJ%2F4%2BdG3iR%2BqDJNHaSuPFT%2FbiPiUsixd2IPf8l%2F5xrBNEnpLJbgZjfT3qp81SBk0Amale07gfuB5i5DMcYWri2sCjtTGm2y3QLBYFMw5KPzyAY6pgGG0t9QJBTEoIihjtrcvpqQY4maYN708MdjrfWc3xP%2FV3OD%2Fared9VdPNfBM7UYOazfDkUJyc%2B7fieq5TYE76bsV1zWn%2Bkhpzak1Yy8G3VGwPTgh8JcJalsKE%2BYNLFTFRae48rKuIWdmPFz9W2L8O59TMwC7bRwbuLRdG3jPsLKk1eWIdktRVmqlYr3AzTqyVBLAUEFbFk2LCJJcax3uYm%2FRe%2BoyOMw&X-Amz-Signature=6182a1815282d8b4ae130b958b3bf1b2ccace2b2c8354e98a23ada7c618fea96&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662QYYDIWM%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T110054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDHNbAVATw7siWZll%2FuqpKHqHcKy4c%2BvCbE7ay%2BjG%2FsiQIhAIBk9HXtc6wfmNUKenHVDSh6XejY7BfeHViUdjqa3yGeKogECLP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxzcZJXOtSK4qmeTcgq3AOtYmnKWIvAQ4xufGhIFu6LlOD46fCVcumLNFtcjBLEOWDEtx4Z44UJxpsu3HNYLil%2BfNK5FUSGrohAyfcyU2VD4Bnb2he0IYlmV7Jt1reoySZ7e0k%2F7K3zO5458yA%2BoEQxgQZ68FxV34l420ff9EkIwhpIFz13YZv6jzC0LKEUnZVdbFE%2FxmUAnvnxCK2xvRhnJ5GjqzRqK%2FatCERDersxKxI7nCtYykOU74I56ElALU7Q2vpWmkQx%2FmFd%2F9559UValqIl92HFQYPmiL5VYv%2BsEKqBlZK24Rh%2BUyKe54ryOegwHNSQICW31Uz19x33cSlQr0fx%2B5c0ohXxk0sKsRO83pLM9rppBcmjezJTWUJ7CmNkPPO09lcOPSgqMEZ2vOTnpdtX8e67aFA4XVYAqfTGH9b6R29V%2BeFkzujsMnVnXKvpVuWF8Ic1oti0ZQxAeAWM3J%2BkIBrmSMepDNgzFdV9mgkIyEe7dWPVKwflvIiHM55pa62SGtFsJrdmnh3otk33gnNPzzO%2FQl%2B%2FERCt%2FD2J7MD36Nq8P3Dn9geR4Tsre%2FbtIbSW3JL9jH3utEUnfjpVNJoY6vGyYgc%2Bzpcfc%2FcbkanAFIPlyrdcmUT%2FK377HAhNvbLFxqAU4GfeWzCe4KXJBjqkAamjKqwnUHa5i3%2B2G2DVmtDU%2F67sWbm%2B6yKlMMCloDQ9UVh5a9xYUm8HnvLl5UoqPNPmnf%2FS5wOGBfRgaSNTw%2BN%2FzB6eueK%2BSb%2B0ueZ8LLveChGHxoork%2Fg274Vv3vKDgp4xfRFI1gLtnqbmi1wsqKSJ6duiSYRH7vCI5Mlq5DxmT6bcFZshSQApY3YNCC7c73d9lbg2ZYLFYr1tYM6WfQ9BFYrj&X-Amz-Signature=03d990ddf4aefbd351a6feed268a05feb7b15cbb018efa4ee128bdd20295d4fe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


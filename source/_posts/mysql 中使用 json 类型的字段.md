---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TF3EANFK%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxFPzEOCKccph1Q7kwdPXiewWnY7O0PV69J45RO6fJVQIhAO%2BEu3VJCYh70hWolBsKBdYOQB5vcfIf15diOjOv%2FAZjKv8DCCsQABoMNjM3NDIzMTgzODA1IgxxRfPyzyudpG0Elvoq3APUDN%2BpsyKw1uNlAVYzJ2RNH%2BuAUeq2IcU7jjQ3mClvNoKcHl43VolR2PKC2WsTgfgqEpbSE5oTPz7Bbxf5%2Bl76uwYFRKd8Q%2BsIe%2BOGRelDSe6ZtX0SlzQl17pvMKzbyttJIpVeit8nLUMagqUviR5%2BSQGt44%2F5Xdj%2BnL%2FoEWEnqApijEqFkcz7dOzz5coONPU4yO%2BEhDwGPewf6DSp1WwCnJNgrVcGT5KQxeTAM8bKliLu9NZnkj1F0PliiwncEl62eYiglvoSjWvk7cDFWfPjnPU2isVZroLQShaOGOH7mTmhaqaJTCcW18RNDXi%2FzDbokulLSdvp7f76K9ERup7QgmUFKEcOD9WCuna6JgfPyZNBlDpyn1CeH%2BJRU1eSx%2BINinLedfTDVHC%2FhwmTtT5r3L4qjO%2F9ofXjvuxlBdGrOsEEekn%2FP%2BE6WzLRxSoVDKdWJrk4IRgCNqRtK%2Bf0u3EZuNjLIJtYMZxQitnk3bM4gp8DqwtuNGDrBd8dZmcXne7TnKj8XBtIz1sCXgAyTDTRlV7cRMsOsOnx6qOIxk6IfRsUzOAa7xI83laa7lCVPDZnf8p31wd7bWw8YBGT4T36TDq5KWxyOnaeHFrm%2F%2Ff4rRwG7cU4NNxxpsda0DCv7q3HBjqkATDzIDSWmdz8D9rYxzyCH4Qm3AhcKeVHpYSnQqrZWH2a40%2BaJ6N22OYtHwyMD2GvZ9n6CWQX33GXP9aCrfb9lvV32skdYuVwqf17CwkY6EcXl5ApNAyhC1cx5iVD%2BI5Zlz0lM%2BmFcITH5qEfO%2BHAHmocCu3WU22DN6iLMicGwYoYGHhGeXGQVkYfPfrTTWktVQY3DM4SRsAm3vfvxPkNOGYt0e2b&X-Amz-Signature=7bf4147ffa72e26cefdb457e80fa2d52a284c7863f98a72d48df5319b6f91d95&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


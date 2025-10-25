---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TH3FP6BP%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T220053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDrAnINkiKuMGMbb8iaviM8oxxFsHJ3NJN62LKd%2BgGG8QIgEGpLfIKx%2FAYPadWCVUrADYzxou7HIM57HpKQHv5P1uYq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDKxpGmwSQXfKpUDk%2FCrcA8aKCF7idn1gn4FfnRoDZ4lzwKD8x91xCq43zJbO4n1lCaFnvNdr%2B1%2BqM%2B21NPCwZAV40iT1W0J0gw7QLGczbpvy09O1vDFGRMLNFAJKoC8wEOeMlG%2BPRLfxIf9kSBG%2BGpT%2BA86ACGd23zUw25F6j4OzGfl7XPN0FtQB6VWc5p5GzMRFdQaZ%2BeEqZIJIleLWN62wefR%2FP4B5%2F1qL6WlHc9%2B90nHkl6LxW2tLyiy%2FDpmsOIGO5I2XzEA08XOZSDkQErocQnSooAZa%2Bcz4Ay%2B%2Fn95UlxxO%2F00BrobEBDAOIpD%2FItWkNFc7S4X383gygIA7cljT3c6KsVqX7zp3%2FIp5l84VMb9EpeHJ9eZzW4xX4fjNMckN5iZH2onwkAZTv%2BIWGs3qJZv9RmzX0E4Gq4fQHNIt7DSL52ZzGwJrF3jZRe8zvCpKYGOlL78eCJ3R6lB0%2FcGBfZMs2x3lJcU2IVBqcnLkf0kX7relXPfC1aDFkjnH6cccyLObbg4aPcSfA3eousP%2B1Y2LLNG94ZgYYNkXgr%2BnCBZBeqeP26xqLBTM30ZF3fvPohWBf0rCrByZ33yAVRfk2ibwvRLiESVuFHx8BgCdQ5uP4TRywPwA9PUXOx4iR9S7IOIOKB%2BMgClzMMn788cGOqUBe41qSk12UgVw4ho3MBLwkVkYfULfaPwEaIX6inWGUNt8PhIENWUC%2FBiV3so7ADfFEdicRGUFR%2FaaEEIc2SduAYCPm0eqL0PMTqml7EVlXkvoZvUU9Qofi%2FbdHzrp1lndtWA0gG7tGps%2B5H%2FJk%2B%2BYdm%2BinxWrUOVzBfCiLu8%2Fw1px9zGk%2BGbHYIhX1HCcoDUVlKKo%2FSfQOXbPFhaoEbpVuIwyPn1H&X-Amz-Signature=cb1a260adefed753106edc461b082590fe88ca55d1f4bfc91ce7b501a10037f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


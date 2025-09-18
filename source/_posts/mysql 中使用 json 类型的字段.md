---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VUUZGBEV%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCIB8c6npNETYnc6vS0syc1yzUnl8QdobfW0b9Kp2uofSlAiAyROLtxaUKe2EoOpId1FGsqeKigpN0xTEDGvB9ff5pOSqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMs9gyXkuFggnsJlUCKtwD7t66sFHhVDYSVRVAjbCQiQWVDkfT5AqAdm3G%2BgdSObsoUmaXgLxs7VyNUi%2FF9aSfBqJj3qezKd6mnoqKQ%2BwksKzTYcnjhonbzNSgTl82LtK4txI8zu0N3noD5tOMfZvQkacAExnKPVCHkWNOX3Iy1ZcB0MIRoAvoOCPvxEnUk%2FUg4VkFmVJkNGmyLEmlrvmOD9KlUzJdRdNPKCvoOde30fUpyVNFhqqwdfCy6ZunFobIxfRWzykJohnUDQtvRSHCc3LDpWurs6p8XD8gq%2BqInEaufOf%2BWnwdCuj9xFRCCehUvokYfaC%2F%2BKgjiD8h%2FEyxR4PfekI%2BoY6fYAaAggpdv2Jq5M2KRayruwjS8gpfN1tjZ2cjEavBpv4qfvq443lJ6UJ6Rh5uz5wlYwiHHeYjJxctX%2FajKPPDtuj4A4%2BBWR0Yu1HMmefys1Bw6F87PBOFXkOL%2BbJletMYWmFarcEo1Eytm8BX3xF7kTg7FRF2wm0RawUB7pILndzY%2F6CrHPEptP3wzfR%2F2Hbz%2BOEdXdvSfuNcBhDBn9lP%2BYHeE8PUQoYe4on3XoNb4EZT%2FQDGFavqED%2B0QV4mqSJ3WvMOghwNZtRjozD6nEfuNthG0b5G7xMYfZC5VO1Pv%2BIc7oMwltevxgY6pgF7M0j1RlJZHciRtvVQZH60p%2Fd2P9njIACBy29y30IZb%2BqEHfXNGw24FHgKGGEDIlBp1kP6S%2F8a4BROfShwN2e45UjftXfDQzXbhgjvbe93RUJcJ730eAQNRqrkR6dAKeM5nfF%2Bf1W3lb26laNx8hATmXXUIGm86mszksdwiQy5mIBLxBAFCFagRAK349tgx%2BsnN2khz2FxGYDAymAXdNCDx6Qqx8VT&X-Amz-Signature=1e82b962b61b8ab2574d3de4b014d197c6a8a446bf79737f8d4aef58306a46d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


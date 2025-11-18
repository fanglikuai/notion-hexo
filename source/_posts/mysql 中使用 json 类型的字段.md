---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SLLL4NGC%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJIMEYCIQCSMnS7ELCinKn7uSujjF0vsXWsBPX3ZPswbFOKETQFRQIhAL06DHsUEShARR%2BOOWosw60KYCKaDRcIC8yftIgyak4JKogECMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzyz%2FP2OmUGhr43zeUq3ANL4RpHvq98Xre%2F%2BW21gzDgyH2iR8JO%2BCz%2BNDwUkIHndWDRRgyeq%2BVtgadcNldZPSkFLWW8rDY31JRCJoc%2BRNO3syyiP7GAUCq4xjLvydh1HN8UO0%2BXytLZV6VkIWpbQM0%2B1cb1jB75vJ2%2FsgeYoUefcCQWpef%2BTqFK6h7HURexgQeW6K6%2BsScS0amoChYna1CHuVDMwZEdtq%2FEl%2FN0dh1EDmBdje8fb%2B9y5JmNxn%2BvcCOkyCw4T0ZvqDPLthJY3%2Bch%2B2iVcmsn0zVMySL74oJxzqm1hWkmerbtQNxCPu3r3BnB65jLzmvWMiCbhMpKC9Oro67en%2FsfNI2lHPlV0BINP0kV3KR1kpLoq7zm1pSc0M6fw1EkOhigz5PvlnJSFkWBmo%2Fk9la8KqmgWWRvz%2FumBIiZ2XQ%2FIf6xbR%2FSE13EJKVZ00i%2FiXdJawySyiGk5vBVBQfL39jw%2BvtJ7ArcMOqzYVAud3v4whFT0hX1tBQCvrtKpYI%2BqtVyI8LFnwj5dJhezm38r9b98IwPtBPz%2FkekL9V31r1Icr6w931jumPlzyNZjdJtuu13I7337Z9y%2FU5iIFL%2BuTjN%2FLd91Dygyh9xg8xtSmP0a6F4gDNJ9oShOVNxVzHYT2CjdPfy4TCmiPPIBjqkAfgb%2FEZEdqOsh7VqKhCoZNe295lT0tr9WlpIsHAm0b2u%2B66K35WFIf7rDW6YOkhjsCY0Iadq%2BKU0eMGq%2FYzro4khz58zdBoBGB98nehBzz1HbcNon2O5R3kku3pEN5a0yMvMaBVBn6Akj9cG8%2Bze9JybIJBgWYE%2FwbYb5O%2FAxye0fk%2BEl0p%2F85kKQOThp%2F0of1nG91loAX8sagbfviPXnKI6JZBF&X-Amz-Signature=279fc1a0814c12e776786f3a03c391746dba9fbb4b2c3960f954a54b9e618cc3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


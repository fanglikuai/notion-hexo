---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFBN7DW7%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQCeqVSCveuOWltv%2BY5IHAMHtp5aI%2BlxN%2FGmMotksDvMowIhAM%2B09GcyRNwrxcmTJ0Jk%2F%2FUJH5n6FBmuGxqICm2GSpHuKv8DCBMQABoMNjM3NDIzMTgzODA1Igz8Gzt5MbJifcLLCeoq3AOxApX7AvOamiZi%2Fuk1yAND4AiyUNiZw5I%2B%2FNUOSDBlyhpWs1ibomfUNeonos%2Bz4SZz%2B7McX8AhLcqTBINWxWKlGyBY7opEPnR%2BwEK3m01l%2BAyEoImydlno77T1WG%2BAPZModJ%2BKyc4dHJqaHUbswAZutex6FWXYIYOGeRDfKUZv%2BV2gaTjoZ083xemVmWKEfsuDYhwBcmA%2BAjcrsvNFrS7trBlVWP%2BsuTBx0TZNPtNnNhxcCZYKXjCVt3ZNi4nyQ%2BSLocqu7%2FZMe%2Fp1fBRGpTCZktM9YshyUiE4S5U4%2ByRyj%2FU4yTo12MQPDlFgMYdLoEWRT2W3ZEReFJR3EdveLvPy3gdFc8A%2FMo%2Bg%2FAiiaG0HE7PIZ4wOoWDwYT2vvuHhab0IWaewNUJoClMFPkBIg0PX3WysIz98P9DgM%2B5aR0LutirjZKqJ4v1T94ziAd1vcPyPHvLSxGC6oFC2S79QtSG9Nlw5THHiS9p3uV7kseqLyhyWzf5smooAKpdOJqW0MENi%2FzLna5wXss9%2B0N8KLhrw8eD3JJY%2BHDCYZLQWA88kP2YtKQfZR3XVPhrDvAGgeZJqZOasVvqR2wJlzKWZXnM3KTRuqJcRh6KRHg%2FYLSBD7ZRVaOr%2FdfFWPtVtWTCc7vPGBjqkAZeMA%2BweX99WfC4HlcV4KE0zQiXtSDfDrhvnP9aha%2ByFSul%2B%2F6sts0TRnqpPa1%2BtK6sz71bgdwMxYRpk1lOimXOJ1YVkV4SbFWMj%2FdDSafrgz0B5GIy2gcd3hTVjEcNlloe6lSZXscwBdm0jrJnqb1RzLO3%2BjcDCVj%2B%2FgAvKlbULr9wv6DUf1PdmEYPShrClyM1uroQ5%2Fa0Jmg1gcjrYGha%2F5ugO&X-Amz-Signature=67747134907b10cf6218f77262f581381a5973443c0114ad3cd1e30d960f23e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


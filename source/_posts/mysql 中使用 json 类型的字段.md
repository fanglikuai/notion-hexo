---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YHBFO3TW%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T190230Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDUJytxeoPOOuGI9vEeWbuLEMK5W7mEey4E0WbO589GsAIhAPO6M1zIoF4kPKs8YUFXuTxQPO1y9qss07JvV6o4LzspKv8DCHsQABoMNjM3NDIzMTgzODA1IgwmSB9oMKq9mfvAjHIq3AO20qJj1Yi27IlDrmFYJ1pVaOhtSrdE9F%2Biqwh%2Fz6J5ZxWXm%2FB%2FRF58nYrNV%2BXbS3vbw6RwNBBG2tle0YjlTduRQcVuqu3rlXItS6WwinUmNdxNnw3UwtZd2%2FbqZsgCrYkFTK0X18vXMTTMwIhOZxZeesatqD8gz0W15YdM9AmuQ0U6Fav1POG3fe2n5EIBPPFiK4%2FCq%2B01L%2FWFrVkBA%2FNQX3ThoqodaCRLDZNZdJz5BOUj649bK4BtbjprHFjOiiyUplJUQG393Df0JmckllOKJuq%2F6lfG1KdXXErnM4b9%2B6gzaMi6ZAuDB2Dwe36e%2BW%2BpsDU7a9ZeZfL17XELeAEujow98g65kD35GV15reLM9m6a7mScolNemPJal9kXpVpp8WVehecLGzwrFH%2F7uEQ0yyqYT1verOJuXmpPsoy14TD7AxSqVVpUShvOlVYU%2Fmvi0kXR7qnecc5f1CfIpsW42VeKymAD8NWpGMPGr6wB6WeKrTRiSsA8t44j0OfdxeNAA0FeJ7ue5lYwlkaMbHHnnHYygtBmcnE2qzCoYPLEPbL8D8rlJwxfiLXUXHSiy7LbVk7I9QSotjPdkNyUDrt4YeDLsXycpS3kjiKJCXuzYDmuQyArt7cqqzTzFjDYyr%2FHBjqkATDkrOQokpT6D5L4i8zLE1zx1c8e9KgvOSyL%2Fe8nZ4eBhcd9pLHN%2FNP5Ef%2BWk60PR3YhlUrvVLSZH%2FJGWYLC1ySg%2BID1Skf2izpt8GNq2XoBKFR776W0nY%2B7rfrO7eOKcPXcArM5rnzDySUbzKUtro0g7rxAgprsEBYJzzJaBIOH01aoYpGkFSxEvKBvH8ftpCOYg3ymCVuSsp3gFyKLwASD1hJt&X-Amz-Signature=cec2c5da561de4d04e6e1b4bbe33ee798362baad2b90c41a7e813871b6be993e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


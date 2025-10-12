---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QMNCCAZD%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T140059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDwlafPsOrx0BcTLmX8J7c394PApPY%2F9rFOVBsNTPii%2FwIgBwJ9iXKP0aNmnTcB9KGSJgxvhWtcbrhBbRAh4MmXurcq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDIKZrXfQfRo6rcK55ircA7qudbeXcGXcKIRKxbj6A77%2BZ9150B9EZWlzTsU3EvE9L5KWcfjWGXAVcMHfgDd%2B8%2FLp6piJ7xHx270K2R3WvJs95SZzTaaao2weFQ%2BPKQ8%2F%2FW1hFjjFom4fwhY90sI12XNDvoSRL1om0eAE2VhnzAMohNGLXNC1b27d%2F9qKj%2BGDssxXhUnggk1Jklp2jsalATeObOrmE42ozPZ%2B3RZijyxQiVGJ%2FFbbyVR7AaLqGW98n6sXuHKIH2KphP3zBiM%2Bvvoa3v1zESJyorDFm67dlL4%2BqoC1j%2BQRREUCwMCM5UVbMDebSV9zadL4U%2Bc5joLx6%2BgS7%2F3qH%2FF9UQs08qcmoy1UDp0zq1%2FEbUDJxRFtTyTW33P0MdLhQjQfSm9Yvsuh6JzNe7%2BDB6S4R56XIgmqvrR%2BtJ5yr%2F%2FleRSjiNBucMhrz4hf%2FGNKdRdipA85zPeYaQm4Nop1p0UvwmTd66OjqsU%2FGimH5%2BCR4TJ6Wdv3XFIEPzZlN1pA2L8QtfUXvDiBylnDcVI8LX7S3UkMZLxrI9kNKWb5HW%2F8mP%2FMqcJGU47qmr08Cg%2FUluTLLeYUXSBU0OncNllCBYz3ueJNBubuE9SaLDG7kFDXNpQ5CE5gVA075jLgS37RLRx0JrJkMNq4rscGOqUBKDNzpOSukaD2hew8VKFyyOgaTGeMX5VE%2FGpNadtmnLMKRuctXouocOKU3k5xXsLA%2Bi5dxcKUudosqpydTw0dc4iE7ivH6wD6TauSRljnHzN9jroz1qy8Hx7R7a1SoVB8cYyYpX4regZ94jyV6rt0NtMyq%2B2NvHbXsUcbW0ulu%2FQd%2FWg9GZnsDF%2FHdukPTmUpbGJ6DdaBn5Jit7EbUlOI8CRojTCb&X-Amz-Signature=1358da7c305510d06588d020b49fc7d76a5cbf8969edabdbef76d729cb2c9a31&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


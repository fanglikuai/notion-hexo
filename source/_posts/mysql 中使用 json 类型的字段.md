---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TYUGVQNN%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJIMEYCIQDDjFEY4kgPfKYqSKDUGI5Y3RH%2BcxLL4Iut3rTAjOOFvgIhAIvPro0DdpvfdvK16OUZz6igDQq%2FIHSZD4sId5l0YOVZKogECKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwRE0iyccaLgimw%2Fgcq3ANNd04pcxLtp%2FLW%2FvkVjJ6ETF3QZOYIpr08gC6SYA2%2B4bj0a7MyuPWCME8vIniLwidbCZZYpWpPi6DRewouV5SAHjN46e6qa45moVTdmVSJQLOHYw6P8HoY4tO2vMoqGDsb8lM2RT%2FEVMAKQylJa5jU7zHhJqk%2B4OboUosIvCCkIEB1ft%2FbFYodSHNTu0adKWf%2Fh0lDkoCSSyD9OkxpQMeQ7vGbQPxrRL31cvMhJTWvceUIct9jifN%2FjEvgXnJ4L7fUWZpFKo2JUzej1cvvofen7nIXliqVV2UwXsvr9i7W%2BXiRSKcWZ29allRe6ENWHbuIlgkHJsnhPUjPSA95dngbR7W%2FRsLj%2BGcKKpjcr26fdYzsuJ3hpS2UTjL8FiP6rMMIqaaCz6COVL0UmI%2B6MacZ%2BAFvVlK6H8eJVCLOD5hzj2mbiYr0yIk18Ey1B3mdB5LcXkCkajN72NfLGIvhgEHROGoBLhZlUE9oyltAtiKeO5IAiTz0j%2Bvh%2BNfMOCjEfE%2BzloTExdw%2B7Oilxqf6V6PIMrdknzzW9OBsrgPicIwv81fI1x2oQJQAFeqpNV%2BjGIGbsFcO3zWC2MOA0IhFgZdXUfY%2FzMBFUCMDGB01mml0umohUUEbScaT00mG5TCg5%2BDGBjqkAYhWjcpM3RrvdFgV1%2B%2BmzCI7dU3KWayZEJv%2F7q6uJ9CCyc%2FW6G03UgpEsyZsWTGByAz%2B9OMV%2B0jaRD6C5ZCCktGrNJnp8U2cYEEkBESz6%2BLbxhdl5XLHE7%2FLY3%2F1rrrQvP3KWTnclXf0sW3kGSccXbeOTjiJxWQiVoClcw1kRQl3oJxHzRLe6EKstfKf3nwDaP3NBv125HjzL4CVYXij9g2tGkOc&X-Amz-Signature=43bb48d2fd3b01e26f34cf924572cd5c87cc89a2c0fb82e41141baf990eb91a2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


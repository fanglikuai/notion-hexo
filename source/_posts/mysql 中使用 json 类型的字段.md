---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RZ2CUTRJ%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T130102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD1f7E31%2F7pweX222zcFS%2BwcO0XUqV67GfLXZqUfEvWKwIhALFbiz2k4da6Z9ovxVPmOM7IUY%2BaChPbxYbyJv2Rewk9Kv8DCEUQABoMNjM3NDIzMTgzODA1IgwpFYSXhgQW1Sqel8cq3AMBEUwb6VKFMhJydfXLM6tkkp2iDXnYsp1TqXYRVvejxvYQHt%2BC0FrCIpCYD%2FgjgpaRZjtrut73dJXaNcBaq37h9g63TKvTmDmo3QeRiQc2ruEB5b0S7HUarz2G9wlLLq3Z3nn6HFHTsLjnuR%2BzUUc0w32B%2B3xN8nJI9pkWAOU5F35fpFt5luY9yymTcv0GIUnAuo01LZa683jNQHt7wD81ysMGPm1MuJGwBKIZqenkKCim2%2Fjl0dOZYVTJH%2FhA697rEfzbUS27KYOLJTHLbk4TeYa0jmxkuA4Yq%2B2mLxFFjsyShQ5x%2FpFJ3AZzojIleqCH8eIh04ATNYGfDmoMRgQ1%2BJ3U61EDHvOwYs61a2zS1Hybtca6ZpVTWTyVfZaQ7Sr%2FlheQOAfCS0dkoytpvrO7TspDxIsWsJmNlX1y3Wk0I1UXKl8bnndZKo3e9Z%2BlL5IDOv7klGTzaoNXWIhd8ZLmLzhv3XwvATiEPAPwx46Y4d1OQvVhsiIlbSSyl8uz3eyKRBHE2c0VBa70NkEWs1t0mrqZ5V6uoJCyh3KnsH0ocVyuX6hzH7F6llFdDbDBfF59%2B3Z0XBR%2Fc8EQG03ac5%2BSlvCvh%2Bjx%2Ba2zMp1mKiaWi3%2FsgMmGZsKUE1XsbzDStujHBjqkAd4jEaGPynx4g1lcl%2Bg02GjngMdwzjXTLT7KEHXC7D%2FvCrMZzdSQBe9uZiA8Fa3oss3m5F5Wnrncki0oq4ZUFsPSRNd%2B0eS5qIqHXxhSC71HS6fQWv8P3dg7suZr5krZdmo24G3AZR%2FgHL21RELAdELuueYBuDGznfxKFKvzg7y0z9v2ax7e2o9hFp%2BrX16d8nDfdBkesz7dhfTF1cDJPlXo3rCV&X-Amz-Signature=bac6fdf2d5d44a009d12b3cc06fecd893b94d4677b29fcc7695255acb8b97b5d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


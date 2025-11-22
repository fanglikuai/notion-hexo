---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7XCIQMH%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQDvwwXWf8jAmylaRwqLq302l06fpcsGJXpS3kg9%2FyQ9IgIhAJRQ8qbBrtB%2BBSNRzYYHCpcjE5dHPjyQb5vymhVQFN50Kv8DCCEQABoMNjM3NDIzMTgzODA1Igx%2FAeiJWQ8sZsZ9yMsq3AOA8Uk7XoPp6izdnCWFU5TMd43Yn%2BkPRjbq5fg8RzIibBSgA6%2FHKcGWOQRdLXgPouWt9V6lvACYOErc%2FwWNRDBOCr1p6mIvOYPnORB2EE%2F9RR9N892OZ7fXBJCOzEkJTLvChFYdPK0m595PxPvJxa%2F7xQhcm5Z57PCkvCVGdtxGOOwk4PoZpderG1wLJ7qVYuEj74UjcQ0tS%2BrM8iM61ADLW0Fe0T4Mf7ApLixxtOHBpBxWA%2Brux1N7pIbMVWzM8m1DeazRniCaWP0QfIR1m1MDzSofoishksGdR2LerJ0dbOGjA1Y9NmLhHcQLMpTLw3%2B0X0a9EafSaukgpkydLycDdpIs0%2FP8B8Z8IRUah5moDHpYerqSydNDWdp0bO4gmfrx3acdLDdyZNtZOxyJ7vzQGyz56VsOnpl%2Bbp0gA8yhLjs630vPuvF1sPXnZbS0D0vyG2rx67sLTaHh95Jy789%2F2Tb6JowkXuPW3HxnegfETP1IjJur1o%2BTSrDGuk5UmqsJcoCxvj3gA9IJj986AJ9eyZc%2B%2FkmmQ%2BIMqVqXEFkkvmRxkifiJiq1VqXUoPJlHc8MakzIWWy2gpb2tria0W8KRp3mpCczM0ooEGgdOuACtzWYX6I77Zo8VG2PazDh3YXJBjqkAYSBduUS1gk%2Fvc07LbAJGmsZPVRlKbRwNxs0WoudkMC%2B2n8Yn1vvOYWbgEyrEGp9nFs8amgzJU%2BqbTKMlZHJIE9A5U12sldsJyvkwn%2FoCZHN%2F2Zp1syVi8LdYLevIuOMhjX9YQ%2BCs4DuE4bgahkm4QWQbpx77ips3h5LNY%2FHSKMqG1SRUILEIQLwHvgm1uX1waSLH4uzxEPVCOad6vpfDzdJgjAV&X-Amz-Signature=3afc0c22515046f55ad04cf624d97968b07ed5112b0756fbd60e6bb110e6c428&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


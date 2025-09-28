---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663Q6CLRFI%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQCtYaJgiBCt8FWRBADUtXflWjduV8loQjKX56%2BLbothIQIhAOP8OGwknFwAikz8IN54EFdFjNF8Mb505e8BtJrFmUvTKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzXo7TuTYtBySvBgf4q3AOD2YBS2drZe5TN%2BphrwlNGMA9F4dcv01zJbB%2FVItxguW9FIvKa5%2FwAYZegjjVNn5N9791W3z60ry0l5olYei2dWyo0U089ScCRHPulE0sxgjC6P1CPj5PkZ5yHo9y%2BRmGAb835SeTox4PvRjpQcIEAPwnaTHOPOFm%2B%2B2UFQCZr5WjNHGEji6P%2F8N%2B%2Fc4SZQktFUOVJsf6GUGmZ133A8WzWpK17lCI1yKFKUGuK35VCp1NPmsTpbFNQZRURchQeWueoXPall1dPgkk%2FIn%2BiDVOQs6y158Fq4DibDtPhPUfLdp76rVL6KE1D8zovfK5L5EaQUD6ZMKMiS8%2BjFW7utkAGkHnG%2FtVv1dm2j82vCg5GuOw9KmYH0bB3NFn6WvX9zZBmklTYkyMiXTwg04AAW76tgpCu0jDKYlcZ1VzXqcBZnJQyjiE8d9f6al8pdCu2mMlC88tx9YsyNFL%2F2LBeOyuCqa%2FOnvN7QC1%2B8zgtWQElr7SXGwaOY5VpP%2FQchw%2BiN0dISwdZP%2FFI2pOhqe6Z1%2F7NNro912BAuzBnfIrhl4okdsp03VC14PuzHlU6TpH%2FBMmJQHufZE95o1qmFSksfmxROEK7s6JNxYtUywS8Oav4jR5p9sPfZ9jj3QUC7jCRquHGBjqkAR6ca2%2BSeY%2BKXIlUMFmpFD6HSbuU%2FXip64iMyA%2BZDQoE9KVeIrCB73GDBJvvzoZ5YUyIWVsGUvAlfGc28Lld4UdZe%2F4Bk0NU0ef3oC9Ad8KOwez9NCEOCjBW3A%2F0n67ffa6yNuG%2BnOilQGAo7AeEyot8QgsfnQWdYvTEGTSy%2FW8nXCU4I%2FUXtGMDX%2Fgei96tPO42dXzbnI1Ugj3pTwz%2BNEAeJyeB&X-Amz-Signature=dcffa4e243657564b1aa38d9df750a6a00da92477d60654879aa4849e318b96f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


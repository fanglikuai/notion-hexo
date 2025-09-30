---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFQJ7J2Y%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJHMEUCIQCOCkuWwwAYwc2%2BtoTIPOZI6Ju6aU3lEj4P93KlKkP78QIgAVQk1ZcIhiNv5SYXmBDEu8yQDhUEGafKRySBIMxIjMEqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFlB2AoIMOx9L87tuircA0sfQJGQvccxWfBKR0jU8w0bFRMFQwfeRam8TdGsNqxKmIzkAMF1UpyTKuVIEiMoG8WZf3jA%2BB3gXwHvQQ8uLkrefskwjSFvylnTjtqimPyuK0i4k6SsQtGBf26beglub6A%2ByHNDXiHRWjM%2Bdi1srx%2FQkuSPrbHthFkhWJIhVW9xYpf3%2B4fznbQIO2zpZkiQnROUVegKyeZ%2FwqoRqT5LnTWXeRWbj4RN0k0dZSRkHzZbtwCzcSK93MukqxULqUZIN3TvJFQeaUqBHaA3NFTTJgMVazpUjMkhx%2B%2Fxkyu38vobCbQrRpek0vP%2FEvgO3TubwXSMJKVZqqjC9ApJ2uovABdt05yD42rak%2BRAfUHEiVW%2B6prnh3sxfTwaJskuaEwxxNNFKBvfzdx8cvYRtRDSYxdwM1W58goe8DtHinJmpHo3RYeiiNbi3VTsQpUn546k6fR8VAHpzRD8jZGxSYHOMd%2FKsM5RJ00tkarMFL8dV%2FME%2F3IGtScssSaK%2FNiIXuTzAeZhgBXjT5N2UdcS21sh3jizNetV%2FYIb2ps6iqvbgXxPkf1hl6ysqESxgEAhoRaJM8PW64qhYY38qb9pfQDvGWiAa6ao2WFn%2FRBeKuLR0KajibZ6VUgh42jyGBHGMOSk7sYGOqUBrGLfC2o4qoJgbW0t7pf1VtOvLnYthuowklA25IHQMWVQ%2FPAVC45zmdMw5l9vJmCGKCmrrhc18Q2qxrL0O5pPmESUlcT8KLD%2FwmzkgKT7WJ%2BZUXUNllyCqkd%2FVHZOUQhKb0tKV279JCrIZmaOvrRMXWApQsFo1fXhLJWc8FqPgW2BWpJYs2WG%2FvpIkle2uwcmMiSDsO590%2F7D%2BHVPTg3xeG6UKo5j&X-Amz-Signature=1741033255cad0fcfdb133b6c6159faa467080d495d85d28725ca7b606e4249c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SE2B2RTH%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T180048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGUh7JoE4eMw5QJom2HX%2FOfZwBmd05tgKacLbBgYG1RHAiEA%2FqoBXT5%2B2C%2BqZfWLkmmKzuBSaZKggDEW3X17sEHcVloq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDAsSCFtQsyImHSKCSCrcA0J81Y9LM2Mb01AjmR6JIz0b7fLeRhxy0NGKi9VuytwRBQ%2F1NqBbyKkjVNiLgdsimgciCy4arxrjHfuXuqzSNwJ0sA%2B4LVsy18AFUZLogaUF1Uszz%2FAf3yPgop%2FjNnxjlmfV9nQovTW3dWEx9RI8tB%2F1Lp6Kvr%2BJPMh7KpDmMQgagAK9e6oP8liuI%2B1dxia%2BJKlFF%2BTCWlZQQQSHBxGoqpVfq3IfRVhi2tSF%2BscS%2F1JJRMacSrEQrpZ%2FzJjQDvFCOkdGzD5GzQBMYuSw3fCGGR5ty8lBwH%2BR3GOioRqdEIzfiu%2FVxtWDRwXnZjKB1foDXEMkHMlIbZun72xS0iYyp8GRMVc%2FWQZOnNVDsMau%2BkcgmbbXo3ltcmjrqqhtYIjQc7L9Fq9tZ5Ew4cwvqGxuUF6S8dbQ0E7q9ONSa3kM0oPvlEXY6eW0ABLZTlZC%2FIaNuO9ofUJjOtFn2F3fOr0CTFPha4%2BoInUuw%2FZ2oKJywsWZDNwqNviw3a3BQJkwafUHwIbDkhnFAqa8cfKLH%2FZPCyCAIjTYHZ3e4NXgwuF4HV8Hz%2FKL%2BwgnvOgFP7Zmh8aZnQ34FgCYrOAq7kmLJiZ6Lh4Ab%2BF6B2Oq0qSUg75qM5OabwK4H4k90YMbjeyWMM%2F50sgGOqUBuvMFmP%2F1PPWaoAqOS2BA1ZdzH1Hk2Kqg5viQDw108rSBw2XQFQSRPtsU1ekSbC2O5hNsJKAjIF7Kh7%2F%2BorJmdJsUMrN8o0G6zI2qiGhoCReJQ%2B0Kt2Xxs5xNmsd8QXKHx2Wct9N4w2Rk89hGttb3u2dNDFKWTQo77ZROSr5D%2BPwsy7nqMz9zkeiiZV7HfPuBMK53QxgS7egCEZkiJcPNY5L1fGQ7&X-Amz-Signature=1daad4915a75cf3ea9e827bf0ae1dd1d853a20ab84d73991b179fa7182855061&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


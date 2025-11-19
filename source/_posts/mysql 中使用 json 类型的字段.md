---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QI5VXCNC%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJHMEUCIQCgN7%2FQjDJLRC8N3OjtWwI%2F8FYqiCfJoJCAOYiIXaQlSQIgH6Hr3isyak2KBo89lIl8dFjTPvux0%2BPzgBFr2t94pkoqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEYcOLBVYj9Gf2yn9ircA2dZm90UrS5wIUx5KAjZ9pU%2BeDllQA3XA%2BUpRq7v8XdpZBJgd7fFYR0PBeiW1CYdRmBQjo4ik%2FJw5zz0%2BGNhCHcBC2D%2FfV2Oce%2BMxFL%2BJnFW8pOHHYzCj7Cw3nR9DPu7mCW6uCahvS7i4oknzRDVFlIn8J88A5nbqS94LW2A7k52jkpBgjdoEjhYizTNOU5LBxsp4Eg3QoOqEYC67kY4Sz4l69hki4DJro5IaWo%2F8PNVQCqiPllmqkhugt90zCzQAoki%2B8xbJNAHouTWn3gO8E8YbAzUbkVTBICCmSOFazz33C985Wpm9v%2BEGdzW%2BbMFAutf5y3dcLzAdJ%2FT3MFFEqHlHwQOjX9B%2F6h%2FS88blvMkrEu8gr2zgLcN4BYsu%2FVnIa1eH0IDjyigAcPanpNqDS1m5%2FNCuTYrULTKBUQsHjQlKD4XFaRgV%2Bo%2B%2FDB3NTtCXdR9BTFUQTSnH5c30Hn3GZT80m8gMzznVzApXNoFqPDuD5cNNCSQSLRHpYnv1GFXQxFqO%2FEEFByWRPJfGp1Ma2jRYkoo1jINXR957P78wqr24TW6UEHXGUnAFw1Ia5alo218g54UD3ykGtpPd1EeOD9dx%2B6wDwGEM%2FKP5MsJJrWECKrEu%2BHGqEjwcZaHMO%2F5%2BMgGOqUBT2gPTGHDZfukkuOd2WzzUC0todX8hCJClVFly%2BRFxHC%2BNTFymQT3BelahBKI4BWowqWSJab0ths%2Fj68T48F9xC7xPuxhj%2Bqx%2BFMGvSLQAN1ponUPSvpjUhKJfSt7UEmNhOjT7xJu6MIm5exSH2Kkac8gnLRYIXpUDE3%2BaxnjkxtlllzdS97CDUNovQbmdMpm8%2B%2B1cy6Wh5NUNJuvMYxibayJz5BU&X-Amz-Signature=17bf268df6ef6f3ac79b4ab7c4037596332a26f0e7fb797c16aa27cfdaa79271&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


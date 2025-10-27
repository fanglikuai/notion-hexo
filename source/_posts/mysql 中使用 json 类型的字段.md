---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WH2QITQI%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T080103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC7H%2BIwT5FGKC9HNwtzin6S1t%2BZ%2B4mPI7orrckE5g6G%2FwIgVduZoWjNk%2FDop%2Blyg1Bj382Ml6PbsUNmXebEtC7fXxIqiAQIoP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFIYPEIZAXhjeFl9wyrcA4gI2hs0kLDr8FR7dJcGOphASR36eN0%2FQtowK7%2B4e7bJgQu%2FB1UJV8aMTuPEUcFk0Xh8U2hdOBQDcerrnQY49pLRON0RARBbXGIfrAQg6tzF3B6cnXguh%2FfVSG%2BXnnw%2FoE%2F0DvQlTjPulEr5aN7z2BrJyvg3sCteul9Oo30YLorVq3npyid%2B3y3%2BAsiB04I%2BSmp4tevb2GZL0qCjtF8qoG%2BVAvnSM6kELu6yMOxHqKi5EnTkKZbg%2F8ABkALiAKIV06M09huNv7%2F39UBriK7L4ErMHoasUsWs1BtpxhPVoFzJvtEvjgIhkFzbJxvotVurD%2Fhj885xir1BdS8bVCJimb6XIZfv8zX4TYuEu4BPYOkxtHgNQWmjm%2FAThfJ2qKrUZZY3AUdZxBffLWR7O9ITt0JvEHIV9JSqUGFCgL3weU6%2FTm5MDJMfqj3KJVbhRKjDsrsvDC1cROzo816YXHEpyhw6e83E9r1xCP4rA8%2Ba4%2BLzxSqLeA3O%2BUTUFfyRRclmadPNbfqKHiAyZRQrPjyIVmr7q%2FFIwZoV840BUbIVWFF0dsswh4zUytR5UF8RtMBAlBPfa6566uEJjv%2BIWaauAbD%2Fe%2BiRfnfaBxlFbbpaiT9%2Bmwx3Nim9PKj7kHT0MJmx%2FMcGOqUBkIutZk2AU%2BYfF97Z7GUcXtaVRWqrxEGMuRaJM5v8kS6G5WcvkGikay9GUrBPTOxqBunZnu%2FicnMjXgqJjShqpfxwfJyljEKCZIdNJLUqPQzQ4V%2BC2bVgS7hh6VtZjkSkIt4x905UKt3pcJWOyqUx%2FkHdxDbx9szh4%2BmL7MhyyfwRsFM0hucO9SBnJ8IQ5cDsMAOrnxWGHt7URNp2MVJTWVZ%2BDKDO&X-Amz-Signature=cb7cfcc95c0f469fba3419796d844cae8098b25e4b98b9c4d9555f255e75acd8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


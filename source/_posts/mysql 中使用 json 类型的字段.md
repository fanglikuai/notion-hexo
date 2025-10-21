---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46635FA3UU2%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T060057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQDQbhmqcso3nYsL1riEDTSnKlDfjDWsIiLkoQ%2FFHnQUNAIgccA0ieHjPZDTC79f0mbIDj7O%2FPqINkvDHfi%2FJe6w4SAqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA2jWe8LD61fPLXvrSrcA5qEA2WJDGtL0AxtEDEqOMEgeXHXXsxl%2Bnm8%2F2IjBdbEQtC1GXByY5%2BwavLpup6yScZdgGoMlAlhzMjmH6wrTH48eLaVZXvEYJWQHo%2BiGHi6qATCBKldYmbpoD9bCrS6nE4xUwZtawA7cXkc1nIlTbwavQNSP%2BJJcIkN1qXjg35ipXB14CIp0T76XHJPa%2BxUODo6Nlc8li4MMIK9OfGJV%2F2yggGQ7Od6hiyzZOu55zdXv1Blgyn6fnB4znHf9UqA36QXBGhhxugCD%2FQc1D2dZNQMkrpfZy6mJhji%2FknnO9gDzvCwZy7vRWqpsQPnloFYr%2F0j0CoT%2FHm03nOsrybLN1iA%2Bj%2BkswgFpvHed6s6wqDFaKHZ9%2B2K8bDGgPWZb6kY%2BK%2FUIOeQ%2FwdUtGqhdxpBErmzbaSOSsh8YQHaklkZN%2B5wvLM3hJCmivXlORlvWqIKmgjHn3mbAUDJ%2FrPOZ9a4zqz0K2a95TIDSftqqz7JgZSzE4HMJXut%2B0VrZbC2Y3wOTcOApiBDTLVpY7mYnAxw54f%2Bk1rhNnmMkraocGX9Aw9O6HyTyU%2BsNzDZ9SXLrTETd%2FmmOdkZJWs7rw0iKGvyFxIMN1O7XM7t%2FEbQNJ08UgOdngDfqJePwmkxtOS5MISs3McGOqUB%2B8yVZRA5cyeLZw0FuIifcuN9aufYOYw%2BKbDr2OT9tGU4rKFGBWV%2BLPrjH7FN8Sy2jRfuRS9xUHZsrbNcsTFfJpoIIdbKWCENECLppKVOzNbgXCgnJsF1Ic5W0acTZoZaIlJezW2rT6Whgdng4%2Bojb4%2FTAkE954m8wXDKcYyO%2BIlBznAZHi5BsSbViimUL3kMvEbSAtxb9oTQp02QMI0PPQcT1RPs&X-Amz-Signature=33a689d1b428c4d43e9aa6bd3e28142c5f477e9c55c6c1b9d2ac8b13487d69bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WUGPYKH5%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJHMEUCIQDG8WWsSk9duX5dnQ5VNcgDdUA6L2etV8XuUO95xi7oFAIgdM95T4T4u1k%2Bpuh%2FfeomG5LPFby8Bp4RaKWwRReQ%2FoMqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFBxqoBIRHimpcqQPCrcA15svr5T%2FrbnBEMNKfjUYVGD4BIDqBsd%2F2tDu5jkEsuHsBJxS%2FI%2BZ3ULOwbCywTPbLu3w0E9IBMX%2B%2Bq4uQkdbLRgtJ0ZuCBNxKeRygk66Ya9ehxyHhdCgK16%2BiDZb7xcqT%2Bkdg8p9k2NojaPdV3FSHdTjdqGCc1ebr3SC7sk7A6bLsclwiivPAqoFmtwwSpDBmuxxr13MHFkKiaHyx3paM7WYolFs7Htp9qhCDgv19f5x9PmPW5m0rcMJPLWfD8hQUm9WL%2BvNgnU5dl1Adp0mYSM9UoKuCTRnO%2FhAUESZdBfXJNkWF9Ef7dNBv0I9slc0B08oxrUYRjO33n%2FdyXbZDCqTHYKm22qXNKeQG9j1LzQWCObraZ46QA2nTrMs2Zh56GTK7QxaC%2B8zvFDS0dh0krfSREsBi6%2BjdQbzOG9oA4ejqyKtIlpongq0jib5wQqiFyGkdDrQjpOrZHuWFcQ2iwAvBzymmC%2FhULInbDAnXaiEakvuvhC2ukhhWx%2BJsJD%2F%2B0Jx8ZTzrHKqzsOiUlM08Met7ULjSwQyx%2BZrHp1PtNe%2BQ1DpnvF84UqXEp2TJwZUi89oZZ4fWIUm7jBqPdRNLTGhqJat0GENxRVC8f9UKkjNfT1jTSA3dUI%2Ff4%2BMLScicgGOqUBEYProqdYR00LhVup8t8FmIWCkEgkulz8CHZvSYya4C8Kcpqd%2FiqwIxmwjhVMRAcmzeItOBFLj5oeTGKau%2FlNHWlyujiZ0u47A2WD7N6UtJPSW%2BDp3nMwBgORVSkYv%2FxI%2FdwKa0GApGy%2BafgwUwdC4zCEfh%2BftEIgydi%2FkLVJkjUoU7LlBzwV1vzgjIf83iRUEKguhiazrTkH%2BWC%2FNeM%2F4ZKrPRDk&X-Amz-Signature=291167e9ed68e756ee74774cb5314c200b689feb216356a924ef7a381fb3a8d9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


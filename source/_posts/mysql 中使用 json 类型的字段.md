---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVQD262F%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T060049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDz9pO0amwxpZD0ErKlXdSufwFz3GkWxwXkd5f%2FGYw0eQIhAJsrJE3HiWFnyLfMz41XzJLZ980YN9xW2T1wWXVnbjVCKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyBngntsbF7JJMNbHQq3APctIwj2L%2F%2BUCktbRO4tQqzjrKP9t0bhPPGjS6YJs%2F77PObf%2BrLnuoV5RsZaZ%2FH%2BV9DWRxUPrfpywH%2Bd5G4wghE6lw%2FX4L1ffJq1AjdIQf23JSw8QXbfSa%2FbyFqm3ah%2B64Ed0gcwb9YAX32TJ6d8KIhjdF220sYY9a1ukLs9XoK%2B%2FoEW77pcCbBeSt0IjBz%2FGxrmJfAjIDmhOGweYJewfwYodTJ%2BQrqsMSPhBakxzkLzSIMJFOgHhWd09MHRR40ncobH7sBJPIjHCRO7LxtRNnWna%2FbDi9rliM4whmOdC%2BA6qlMTBysBxunVQrKRa5oGfcsevmU%2F6iME1FCT93mSvSRBHtRaMhzItgCB1dR80AH%2F%2BDnmVZ2vSZuA29iJSWVLsgVckqgdBzFb52WFleMyv%2B24k51LPuP3qZlcPHOhSg9plkoWHYTsma%2F01rN3UYs4LqaZWqzf2zqe3cGjiJlz%2Be9mbgRIaXQquCDkVty1NpfH9keERGdaErqMk%2Fk%2BtuxHGeXQChSRf59iFn%2B2xAUqvI21Yp8Q5fFcYmmyH%2BroYUkeOAVBmqb6sgVSCgW%2FZHSCqHRdmZ2LKi1XGy%2FwmsTY2tYc1jZnD%2FyJVXjMFHx3cZmJgMm39cV7twmvqG3KzDcuJ7JBjqkAXgypXtXSdSZ1t6DKVSCeK%2FPNP0JVwenBAkxRbtLxu9J2u0AzWIQy7n5XzOz8RHLryYeg%2Brc3eJ8BHtqLwHpbR44a2PMbvjZ81cloR%2FX9esnXqaGQHoEnYdJwqW%2BCHRvCKW1yFJLkWZPPmIVP7pv%2FciDuCDT2BVl2YHvSrvUWShjBJTGh7f8NTluJiZoYxh%2FAFN8YJj79M9TxWl3IKbwfv4HtWdC&X-Amz-Signature=45d47863886e84b73992237dcf68e4bdd2ce560134b3cb1242e4790277b8ee35&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


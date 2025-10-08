---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q243JJVY%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T010051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIFCahMuBUkOaCokFYU424ZMcy3yos2Rj9ups%2FPgCqdjYAiEApi5aUqT5GBYC7Y1Rmeqp1QBGzylxVhxVTKUUamhGGYQqiAQIsf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC6GkA9aT1OGTY47FyrcA23BsUd3z6lcXdbyv0neHi8eA1WiV9XUJyL6ML%2BRCY66bgJt%2BxTwEl%2BmAKPMKbRwouJDQtgUEPdM%2Fl06BBZU5M4tIVTyaFxIryz48NcMiq7EyIWm0S1bIbjbap6PtxoZkGxpyU2dcFuwxA9S70Z%2Fd%2B6W7cTD%2F4qBSTYUuWre5i60oUt9NSDPp1S3%2BiUQ1HjWPXdwnPoW5Omm4nyh4eRF3K%2BT7zXr3tq80lmJK%2FdimMhWZSo7HePgVXRzDrfVsftrczQajXbXtgBuBqwJNt7eY5MKjh%2BoJMmu2PX%2B%2FT%2BejFRi7OTlotkuh9d%2FwQa4j%2FNTIJIeKyDTrvy3mMm7ILa3baxeMkG3kVN3uM4d6zbbIDxsy6cOF78pk6R2V0%2FnT8gbv%2FAukHx9SS8lgt2VmQPCKT7yHdBFkbeZHCtHtMpWrjpgBSoIK0%2BOdESfYugIewFAui7fhp3dC5E10T2d1e5ovK6phIWUZz6I4xMutXEp6TWd0L%2BhxBT6EJH%2Bz02tgn6onOh9HOgXnYMdUfB%2BGQxT1ur6whd6DiP6w0DdX9b4%2BFSu5wL3JYfJaBCS6i52Ts9pZUWri7rdG5ohdNZWk6kwpriFlYVaZ0%2BNxAwHXdxCRlzeDSM%2BBQIyI6SePPBvMMnQlscGOqUBX5puYZ%2BsAx5JUnctTdKYfGKwukBdP8lAVAoIwqLXKjg4hdE44TH%2FmVaOfOa78X3ByS6%2FlUlARW9oLBqnpCSutCglJ5EsiJKCAozP8P45uaXsUP9cx7OQYxpVqM68IFx3%2Bi3jdIeCwRFVQE1yO3BRneZJipOu6XEft5S8ph3KbCFTfmvjprhpL8GCpap35um9Fx7Qv138kfZZYLTo%2BSCFqqi%2BObEY&X-Amz-Signature=a83ea4eef0c9a38c6a4bdf2ec600430ecf1e93c077b7c913c6a1b88f8bafd140&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


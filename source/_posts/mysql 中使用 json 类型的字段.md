---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SIM76YP%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIQCLrOlUH%2BPr3Kx9YhcVA2FzNnSEZHtoDDZY%2BM1OnbohMwIgV3CnSA3Van9lGipnHLHIaeg8dDt5Ddbbe6MUYMgaTTsqiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBHpzl2wKesvyycHVSrcA7fmb1XNcWIKANbluk%2B%2F7dZKfxsmqnn85XdAGv8IOsjurm4TFSf7qYoUqJPSnHYOEnKtgE%2B24GFzfGibYRW686JQ5vYslzf9ZBzH%2Fjke8%2Fqdx1EwHMCMYZRA%2F5XscgxsfndFHSjo%2FS1VQlGplgie2mMhkwj0aXZP0Yl9FBeV%2FrpcGZA0oRBfBlmWNNhr4AWaKDtdjCZwMUl1IYn03jZYJbWXQpxq0jqY%2BVfwMbwoIlbGXYrOkMr8r4sDZuHQ8eF9M0O0pb8p2LuTXzpGJ3NilooRP5GvJ%2F8nYWluv4r8B9MUzdsqgiHdugDGeh2JGhIWu3UbcuME6xqjoxzAy8bAc7n%2Fi7pkks7Ugn4eRQxe1lznb%2FwLUaGUnNXrC57%2BIhZSHWT3rHJ8HN8%2FuGrIvk2LFWDCs8cT9%2BkTk%2Ba%2BBykdB9%2B10G4tZ6zHj02t%2FSi5XKcRHz1Vd04z0mC1%2FTI9CIHkapHS%2B954IvMeLV%2BUJ7wUDGmCYzs5q7hXVUWZQAFrtYSs1GhD3TiiG964Ef9Igx02xttAKtAEZ6cBoGDMrobWkMWjUP7CXYreuFmuufw0cJnLkbXdort9GVnKTncpoCPrdwjv3428qHIuPyDswxe4qXu0j5YEgZRCOUAHQ0DQMM6X0scGOqUBhafGMwLVF1xvKwMsITa4E9qmCEpiG%2By8vZkoE8coBzSot8i%2F7ktkAiBUuLgTkXRxuTQqpx2455iEWOhFHJJB8p1tR5%2FocGcid669Pdj6TPV7wezV%2Fvfa%2FRQeD1zh8UymhLUGZPHaESA65EEvfeWRKQume%2F7mF6l9OtV2rTDVYXXieNSTnLnityxJB45CQIy1ZgYvhxoUsxNYqbHOk0KYPWFj2h%2Fb&X-Amz-Signature=dcb36c6712b6818e21463a1516c76993c357f177b78ab746bf272093ee2b5b98&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


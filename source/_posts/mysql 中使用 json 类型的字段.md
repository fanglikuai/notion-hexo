---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PZPHIXY%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T170119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcPCZ3oySTOP9ZTHAte%2BRsKhlszUi2pwUuv45v40R6SQIgR%2BNn2b%2FccTdR5GQUv9ELqPanwOYpvMvsbn8xjzBM4uIq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDNXXTHvFTcsbiaxC3ircA612EMTD7JjiHgINAJH9SRnJPAKyxwABSSDhQ0oMr8t9C3NDM%2FzKJTgasj9ywpsiNEBefnFO9tkWdgwFp%2BqBJkBazaIGbo940iOeMueXNqwnQjL%2FHQ8B7nlQ0ch51D%2BMzSIuWWrnBnCXwAEWawner4DXjy52tdfDlDC5DqPxMyr9447wX5Z5QJb1%2Be%2BUraxmJTY%2Bw6rzHr1YAYhqe0HqjxxUyxeV%2FACQxCT%2B8cIuGLw8TvL47pKxeQt673y1%2BjcFlen5%2BJBLZl%2BRKKhGBGN1sI4K8gDvRr2y%2Fw1d5NlGvd43vjtHLmiTWcvGzRP2HSpl9GW5Yqx9p1nc5TJYPtwMIvNBzcmBOfC%2FREUirXlzM7T%2BJWyrgj%2Fh5xyxg3sBYcN03oFLfME8XPjPZBJGLPiI0VekG6I%2F4IJdsmyjLj9PBcA5OtoiAakB3ZIWtZnNqVCN2AIOIZ9a2j%2BvBrKEKBLXbkr03n7eW7KTD2A%2BEUxY3daR7W1vy%2BwDoYiuklQCbNQXzfyACZfUJYPSvrDmTJWoUbNwBxacc%2BGwq%2BLs%2BCRGtX54jebPx1vuppBZ%2FmeakVgBhMf5w%2F6MwcbHxk%2Bvt7UWnwBJ5ZLA2Ci22X1Lw8%2Bg7HhLA6%2BcUNr6v2JjhG%2FKMMr3uccGOqUBRs5K%2BEM%2F8mwhuRl5WhKmbDbz4VWO8KR4WKeuwnScsVTZMjDBAM6AHvxK4Aukv6oOdTvBYlm67Pw5hwa2nSL9OYz4BY64DCVmhItK11DBkyXvJtICxFj8%2FjuM%2FkQUg7HlFkPbg7NWlQu7HwCrjj7ar7lBfmW7%2BDicPwVGj2zs8o4aMujWFlQ3h0vEH4RBgi6ulGxmvy1Msf%2B%2FvNziq3pBsVxBkKnK&X-Amz-Signature=03e3041093732a30ef3c179cd3b3e44c3aff937c3bdb6ed5ab782fb5fd36ca31&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


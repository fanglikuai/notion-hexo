---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S6UA27HN%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T230054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICdbc489tu1lbR8hNdeek5xNMffnLeTYUp9nzFdvTz3TAiEA70GspxJ02NjnSEM5uk5qYi91FGh4hWcpUIRhXaCST0AqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCQNfuqDicn%2BoKkhjCrcA62Z8uSBAzIsQ8gw%2F5a3kYqPCZ09Ho03X6xxQnBMAttKWlRiRMn0WaPs%2FwM7sLCgsfvR3NNbDnvqJ0NzW1e9dl1ZTVcyAHhK5Vb6ymYkpgt3u3bpiDD%2BM3fM%2Br7a6cFcpIiJAgYfi2RWEV30Vm0GywUbVq09Gd4%2BZsiMTt5lcMBurk%2BHnE66lgR137qBQ604io2Da70U1rXkgi3m2GcLUrLKMUlt80XWel3RQYtav6cE%2B70Sd6MofB%2BfxTf0QLoJlD9ymKNQi%2BOpNvfS7i%2BtZL%2BNGdQh75CzaXc3wDCMav3mIYUSe%2FBOADWjU6pLoO8pWUrd1YR46XDWRjuw6UbfWrKvE51mQfHegyD2g%2FDg6I%2FH6nn7U3zibjbFhrySbibEdZj8lE%2Fj0RZeT9v6KQ3VrNaD06pU50ALyPrPsLzkjoREfmX503nXZPvEKX%2BqA3%2BUBAM76yShV2LN5XD%2BDcYVPgeUb2H0u2dmEgxjW0Q41rXAnKt%2FSXgbImMBv0VPAUIF0%2FWTZuuvCEGRV2tlTb3KLCs%2FQKdynffRnnJGKuQZuUDsi%2FCa6O0G0X%2BjodMtEW3cDjH74opnZ4yexZvrYHtqkpQa2dq7putqZnxct3OcQw7W%2BvC114xy0NaEr9aXMNOR6cgGOqUBcrT8oE7cI3qe9gVsYaebYj1sro4hjQ2qh9st5ZlCc4T94t6OnRzlE3XFI0giLUW%2Fw7Sli7LPC0u0nS04JxIhMdddm5hhu%2FysqYwza9ho9wYrsw2UlkTPIj0rNk%2B%2BVb794oooe73vPKbZ5H5OGHCTm8XtI4IcC4kYdII6A3OB78sW%2F7jIwXX%2BYJUsccNjtTZqGaK%2BFJXncxiTItVPzXJ4O97cXfAF&X-Amz-Signature=a9cc8a4b375a0dab5ec7da6ede54945542834233a4908daa01a70d5fa4e22c56&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


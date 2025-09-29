---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PT6LD5A%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQDlY%2Fr10FR9u8w074i3Y08ZD7MwCkAhw1u8r5usR3u%2FRQIgC06UuoAo9pfnYDJfBf8j2HJNt0GjXfNDXcQByi6gT3gqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGjyw7HMQsCM5nCBlCrcAxd0z%2BMbGYoiEhWsU0x78qpntnWltvviOVPa0UMCJWY4SnspOh5WlB%2Br2TI0KyO9LUzx9S8z3Ju7yoDqVMJJLHWd%2F%2BB%2FgkSJmSvkFMRjmbNeLQelN4%2B0WikkL4L%2F39ne8VxsxuuGDF4D4B4avFEQo6oxFJNYWzGbO7XtuTo9nkShba6Cip6cp0BNabGzWoviJFx3nM%2BVnHKjTRtBXzGJzKGgOupq17wJ9FASvuNGTfLknqTgT2Suv2ezoVMn9j3Zr124E2%2F77MjNoKWQ0lH7LjFQwUgMeW7g6pZBxwS1wtqffuvZ%2F%2BDBtZPQjTVq0cmaTm0fEs9k1eQtBKNrtyCwZHesdB6OJFMxSp3%2Bs3%2BiJwdtWFpbYiILFAI4SFOyyA%2B31ozx7TuE9B%2FHdjEiVB9knirGgGIXE0F9LQ6McQUXvMJ1yyot6vyhebLkm5WwUweu6KLi2n5acHxjNk6mP%2B1fWe%2BPKnGGorIYiF2LVvKNK%2BKqDcs9E1cO36BXVmifeFYa3C5v3Nmw%2FnldC68gXbv4KLu8AH704Of%2F6CcnoiE2phXHyh4zvPyZPSTBT4y1LrM9W%2B%2FFmDJmZYywKk6pBDYzwBxEODULM4NS%2F5EsoIEG%2FwciYOadrtz70k%2BIZYNCMOar58YGOqUBdfwsTctfC%2FHlC204FeEW6DijeUV5T1Twl9XiYjvqSWe2pvwCwnTr%2FvTZyHUdMbnDeW5kFlelan252kRQD7DSJzTddjR2AxCLLRy6I7AmvjXF7R5q6CBB9ObZii3HcAOEN0%2BbYHUaEmICCPKFEhyZ3OCEA5pJ3HtTAQSSYbLA9dJsmFbSnsikfvGV4dYll0UMJTR4Jo2kIAUZyCD0Tn0exFXckUV%2B&X-Amz-Signature=5875cc830730dfb6c094a472ff08817e5f779937da4903d4becc45afe9505938&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


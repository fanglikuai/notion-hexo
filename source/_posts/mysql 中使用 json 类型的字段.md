---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664DSSBPJ4%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T190140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDCfey2JBuRZEBwjdS3vZJGSK0dWiSwD4vSgoY8neDI7wIgY2pgbABkdaf7pDQyqXEGOXA9EghTUo6GU5YcRoSBm1Aq%2FwMIYRAAGgw2Mzc0MjMxODM4MDUiDPxBoY5cI9aO4wc9rSrcA9SMaeqbdAQCJKvXoTiAuSrRnwwBSv2L0O%2BmPk3xHYhUbJoZnvy%2FucfSHTLBakdWEBP%2FrAeDVyd8yTUV2DJxWcCB86e5TeDmgrc%2BRK2sjOdSbCEyHewJ12QGq9SyQVizq6Xe%2B7UF2KU1YST6I724K4brUbA33Gi5UidGuBHHiwxOCQqSPgt7ktX2NPhvSGIbYE9DBhLAGZgIEshKj023KBrKr4T8WEDAFwBddFwGa3KfKPIf1yXZLmrKuU3UBHYpIcC34i3iixDVsrsXGejOjnWm0GSployoQTd3G8XTKh2p1aVfK61eZ7esFhzyX7xtJ8SHr7vVijL5D4gCZcmMDARaXct%2B1wvSaXqoJMhaD3P2tDdkJBuu66kh9pPo2V2qL6TsINWJ886dI6vWjNI2qj7Q0DYVSDSjedpCXmNggCxEjO57uix8KlE1AiY%2ByOuIldqm3OGczt0c7xmU02n4C1990p4NcJa8VzTFM8sFsk5FPvfLoa0AlRDklT982JdpWDy7nQxQqaDmn3Nqdh4prlwvGErogfwpgwJFiB%2Ffkdxi90EWZDkQDThLPGWdvv2MwqHM0LAicwsqOAwlSxhceH4UHvROdg7OYjDdOTT%2FLs6Ji4d5b0n%2BEXZSfKKdMMuPhccGOqUBM2npAw%2Fm3Yi4FOOCkGRC0FMgRogFt4Wad%2BBBb8WGK5NSmhmBFzsSxzCtmri7KwP5BigSbd98Y%2Bn6Q4qIL2vXpTvD6C4ACDi5X8x%2FTYAub5NyDct7P8fUzC7pMHewU5BxsSQdFpje9n6lIXm%2B%2FToa8dTCgM779d40tgGo9QRBPBSw8%2BCDfwVBtEaigj%2BKR4aeaYHtKPrnfrt1JMiVvAwYP%2FK2Adav&X-Amz-Signature=95ba4b2750a7de8409113a40247ac492685f58523bcc9226c04791a79d8d62b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


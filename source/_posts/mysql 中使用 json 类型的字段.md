---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QY7PHIA6%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJHMEUCICG5XT%2FMlnrxgMwXFpaz4JVsS%2FBWgtJxPM4fyNLXOFRfAiEAiYbuN2xxSU6Ej%2FoPofCq%2FOHfjxxbEVsY3Hq6H0TBHeoq%2FwMIPRAAGgw2Mzc0MjMxODM4MDUiDPFBHITbV%2BHyts6M1ircA%2Bwis6T1QcR2iv2qKTRfnBjKh5MJord%2B6aNSdl62G2pzIlEKXNb4YYCnCb0gHPZ5GsOQsptcReXw1FfJboPVuMJJFuxKCcQansCAShVu5VcRT%2F8xDrQlw932nkzJICsgM2VluBa6XSH2ETqcp8kvu7Tj6kgZJX9QhD%2Fwgr2CUED8MXCwicfhalNbCst55IG%2FREPpvnerVA3l1xpK6pD20su4VUZnd2si2zPL6pjrgQZRcW0tq8Ej7olb1E%2BEOS3u5MjBUFwrbmHBrBI2utzZqN7iKCStT1yRDFa6Q3FNsMVt3cYXojLp%2F1OBaG8JFpzK%2BEYwhQ7g1Vn9UuQMWoEQKhBOIAAbet00zOMHgJIr9fW7uunRlKqOUgGayYVJ%2FmTZ7AhsZ%2BHNJLvFpo9bmEZab5SpbPBKnP2AcLndw4b9W2CAXS%2FzUeQ2PX6bSpRGhE0vTV7x5U6qUlXbDwc3Cv7sFmZN%2Fqs%2FnxJPsZee8dnxKoO1DIIGm1Q945o4nYBETaVygEiRaWnDQMRRLFrh4YraSK0CGWy6%2Bpa8665JbR1ZpwbtEX5jaaV5HG1C3Q3vC1xpuDX%2FjqZ9Ry4QSFPckl76isHhKf%2BEJi3PZkzL2D%2BPVwxwSYtHAdLV7DfTk2yeMIS0m8gGOqUBKxQGfJvHWibIlOc6s1MLOi3%2Bkv3VGlqa%2F9XhnmG4quUj6LFt28OnkPyeSe3Rzw5uXvU%2BjL36dk1DnG4XTkv%2F6HAQgYJrLj67aZ5NBoPTIrCffI32rcTA1xOk8LQ%2F3cTlpiYbZXyp2zYZSQMGptefMuUXDVWqLH9kuuWcytbJcl8uCUeGJA3ZMJXjNelbhIiXOgLtmrnf5SI%2FmsTCcmiMIdzrcAO5&X-Amz-Signature=670282a08c59800c9e5e995ff202b77c58fc67fd557c442b458cd3e4601fd3d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


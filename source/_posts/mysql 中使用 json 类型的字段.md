---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WUVMTZ7S%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T220048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH0ZRZaI0Gd97HjsuOaJRRN3UyqfTMudYj9MEIcsMYSoAiEA3tYNRzrDBqUK5Mml8j%2FdzgL7XCbT2%2B1VsZI3kR3gNawq%2FwMIHhAAGgw2Mzc0MjMxODM4MDUiDLXgnJyrpN7vua4vuyrcA4t7ubySPt50NDyn4YoOid%2F9Mi0VkgOP0Iv44R4FhdwvOFg5L9jWcwDinnvjlKmFUVonl3KZN3ycWoHFRB04TTV6D%2B8awi0rHkgsOtmO%2FlkySpV2YnNGcpoq4sPSUyR8qfFkyrG399NMSukWpBrKw1kaOWc6b3YR7YmreUrEJjbiFj%2BCCdFpJg%2FIquMUwT2hFJf15bPrCt%2FzxubrAcQ90MBMezTpHPGIWdW7oIvDbQ%2FamvUp8m5bNSaKCzbgMH7L6%2Fg%2FiRatAAYUwrmi%2Fs1BXYKDgL9iT1uzeWP6MITrmsLTxuejRH2FYGfhk3fcO%2FIO7m21JgILdIow%2B%2BXLpalOok31XSl6WeA4jcI7m4k6uFP%2F60uiwrUSDNKP3chww2ohg6XkumV8JR2JLQR2g5XzZDTCUelgloS4av2h9UkqiweTu0oiXYO%2FqEJhCp0NEoJR2iOg%2B1tikgGs7uaXLcJagscyv0qVdp9kkHAojOYtiKaHCSehP09sQS%2FtGLO07RWETsJq1G%2BBAZkqFH7IUfQYep79z05mGZQXxb4loZmom60mkgmxQxuF7AYniblNkDY1qgHZsny2%2FdCDreRjD%2FvbNajQ2XsHiaYODJfT38DfWPkupfVY4%2FiwvNRS092xMLOr9sYGOqUBny%2B%2FZ4IAkeyZkRLYg6GpXfcvhZx20n3n%2Bct8kjZ7UdXZ%2BNU7silZ3tCgEq9gsOdxF0%2FHecO%2BogsT2fZSb3md7e651qCtMjzjuvPgLSYqMU7%2BDs7KQ5ZIgLCVHUuDwDtzaZN9AeNGBS0E%2FDedJPMGAA%2FVG8%2FXMEGGJbq1atUTAyR5ksPD1%2Bx8mEtaJ4gQWQkaiQdvQn6nm8cPP%2Bhr2i61Wx%2FZmy17&X-Amz-Signature=54804fb10630556e69d8a5ffec8c3ee8027477312d984d3dec09274141a0569c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


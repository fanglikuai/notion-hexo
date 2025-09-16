---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466727NCKGJ%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T160043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJHMEUCIAFvFnyFpIwXUcoQzzSPs2vZCrT6y3VEkw%2BXW58PVDB9AiEAvpIlh17qaNtgJsJWGuI%2B%2FVUcDl%2FrEi2PaSmCIdqvGrQqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHkC8sMtFnJtZskChSrcA08OuxuNTt9m9SnB9he7ihajJFP6OHxfMB0bnE4%2F074dIgQmeePDqsjbzNsjgAWmXyEZk895F3Bllnf%2BKrv4DPi%2FAdchqy9RPqv3ng5D5jR3JTeqbY4qmh25sS4FU67fCfQ%2BEoKsG%2FaxMfhycy3i97vo7ID%2F2NCFLgcfzALXu5JAARTpE0QAy8XSxaE5xedBK1wE3wlNxIqlrIQsyH9OtCNQVGF4nKJP8adR0SYQZzYeoWzHeZ1uIGt%2B8OiMTffcpDTz1BUDxwlsoqQ264jx5zMWmLAlzCOsz6POR4vO8DhT6ZO5y%2FyarPkjO9wkawywCqjBbl7T2Ka%2BXihjAgh9kFsoyGZa5WeggENoQGFZ2y2uzpqVnHn3lYyqWYTqXpDtjOxd8hrj6j8rR2lP4GgDpMQ%2FsYLeCXd4bEEwz00ZSmzPKMhjx8E4PlxOBK%2FqaD4BKP2bVUKggrCK8ILiaLPwgDqxfRs%2FFu6l%2FnZS%2B81SJgmPHlP2atl5Sv5nhoSc9YE5GD1YZiSuR57wMhzIYcjgOzkTXIBd%2B3%2Fi0kgbxFPtXKfEYmgJmTMXBg1EKLBiqIJRtqXEupxJ%2BT6we9eCQcBX3%2B57uF8LDPtInIcH8XXW2jSMC%2B%2BvVujO5YWs6k68MPXppcYGOqUBHs6%2BHsjf8XT84OqnxuwI3Bx2Z0t7NyPet77pYzg%2FkwyObx14OB%2FqA8XHp1i2X3dZVWtH19T51RM9iCwzjOAaUDgHfTcw0IU25f6mhv%2FCuXQv55fopdsRQNcHOK8ivius5aY%2FLR7aHeX54pwpRoog9H%2BMb%2FhSHQcozkrE4ibBintr1zVDc8z2TYhCnQGUZhReVDx5GTumWg3HD4s%2B%2Bzx1TSALBpBM&X-Amz-Signature=6c565c9c9cddd0633eef427353000b508c54419636d30f598becde6c503680ff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


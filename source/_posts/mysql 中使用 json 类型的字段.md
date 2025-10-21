---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667ZF6346%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T000049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIHo01tbP0qIplcDAlwVcoolzCFIuugzTK5C%2BBOCRkunFAiBDnbPjaKCap0RfxeD7EGPaG6K%2BjbrWj0v9wDEvrYwvCCqIBAj5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdsrnvyB7m4dZNJhVKtwD0HIT7qFULfICtocFzT3QT6vprfxg96F%2BWBL8HyBy6uVNgpI9nHVkvj7ENkD6LRuIFxqiXOrXw%2FioFkYLtThUlKPQoSIbbNkIzeQ5d95rqhjNp41FQPqnemR60Oi7tZmf1R1bdkBA%2BEjfRIdv9WJXdKFGCo9pI3N843vnGz%2Frauy8xjsBs3tR7GSMy%2BStWRyXSXyngdRdLcUqc%2FrgRZWbyMqUtvqxSZhWkNzEDFCLRwiqo78HQ85UTaCSGDb6p9K0DETXChKiT5%2FG%2FJ9SUTzp%2FoTLqQmmIxd0Zk6z0hIK541xIy%2BGD%2F%2BgYx5ZFbYIQkhfyUDAHHfv7vX%2BXOZdcSFhZIy6X1f8HEyMaskjqWmF9iNz2ystuN28GtXFM5mkvYHVPq%2FXW%2F5k5piqpgUlYrsDy9ctEjS4eRoFWcABrckphz2kdB2m5l2rvM5JNUeBbMfeCpgdoD9Vvr58Aj6NHxVZyyoO6EGE5Tq5J0M7J4or5VSEJBDJS2vOBGBAQ7OohA7AiyMGZQdzLHDoN4bzPBfbvACR6LBJv8duUuxWcfOPNDZKoUJamZb3LLUdz1oydlcorZrBUlxp1g7VnzOJKskmKhwrL5dlqsMi3wKBEYYSI0A8r1XroXmD4PfhIrAwgpfbxwY6pgEh16LwRNCnz4Vkt9XbsPTTEo6eCY7%2Fi5E3E4yLT%2FBu5EHQy6N3vKnZa6ypsSRFm4SpeoDIFZyFm18BAyzFpiDukh1p1cR3a8kbicgdv92e0Uqn%2B8ozO4sVwTwPdYzhXba4D%2BSMObgH7dIcDxrwSovjg77NNLwoPvDRFMj%2FuEbGNbccvPSCk7c3e%2BGsRKyRUAFRLeTkoj%2Bmob73Gh11nY9ahS7KYNKR&X-Amz-Signature=ecdb99683916958db7f465747e4699d0765714d306d53251a16e9a12ee47e001&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZPDMDJN%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T090055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJHMEUCIQChzKR3YWhJ37EJzg9aLLXf8RECQw1khLdSKjeEoWpnEgIgeX8u1XzKfH4SE2YbLozn2aRvLluD69lbDkPY6qcpBcYqiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDABHndnKmizN%2B50uECrcAzuIVgT8%2BiCjw%2FUd1Hd%2FiQVL03Regs6aw6hLCeCzA4fgp00B0vuXMfCOguGucyCWNGPD%2FPdcanNmYfUvskkbMy%2FwV%2F3k6k9HzFEI3c11S49i1v5HhK9JRqblECXuI8D%2B%2F5ezPVp%2FFBFHEVgg9EURC%2FIyxoQayhruinRRZAPu9f1GtDt3%2Bu%2FvrBPHSFSn%2F31WPinng69y6FsGveC5fOVN5QvB8U88PUcTCO%2Fv27UpgOt8IRvuaJqOQVAPHdAYXjUh7cd6ehngjbsTxTIUdvBWQiH7l4swmdZYzPFhvSbt%2FErHF7TOqPUaWHDQ7UuRbwPIjVv8pPECm5I0xGbw5gZg1Kj2eYsFfi2T9SrbZo9414uppxTnzVLPgrf8q2dW13cL6nRMzdLX0LcQ6T%2B8n0l7r8oau4En3g8N%2F9AQAZlQjebTPX6976rIkfRWwkYfN0YG11OhwNRX8Sxvt%2BxdQFB5W4ckkFCn%2FyzHLPlYuG4bPyw0kn%2B%2FilRaeNlFpRR7IbV2sniOHQX4v8AdRAAha8Cb6dmQuxmxmG%2BIAdQkUs20ad3Are1mZ7HUKP72sRpVb8QD8m2kIpTlja0fMx5gZo4Tpo%2FcqMdmA2KIAwlV%2FNapx7r2KjcEWQkM29NhuRuiMI%2BTk8cGOqUBvXSrsP%2FQpZhYJE3GOVUy%2BCmcRg9fLxGONWMUGUvYPk%2FEi6T2Ds1X%2FSZLoVhIxiYvXGTTfukNZUM82RvkleluUELVjBDimSoBYnntvEx0dRHHJDyTPDrTYhsx6wVkpRuNuEOdJYUy4DjC0baoo5%2BfiHLntXRPVkn39SYiytnJx0TXcdzUC6NvFJ5dOrCdfv8FTY3Tipt8IBMx4QsOCp6gdxr%2Fw6Sv&X-Amz-Signature=a5ef9442e6d9be19788979b58f5b6e89212e9aabdaa711a0b5ad176add522fe2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6WIGS5C%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJHMEUCIHq6Th7Rm5WMONTb0aaBvhzOm0S7tlR2B12h%2FNZ5ytCjAiEA2%2BWxHnDaXuvzjPmPRqj6Uk%2F9Hq%2BtC3H4BVUZgONE7FAqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAxpj%2FHkWSdLP%2FUo8CrcA9EJulhswW%2FkPAThNUYNNMsh2j2CL1BhGHEdH3k3zf6aGpd7B0xUT4WcY1sYOmned2yY377V%2B4oljs2DDnbrM5GA9ymJLKyYz%2FTV9WjljAZeCooFLJHS%2BSOgcowAdvrTv9sTlzTH0jZUdHeitoM5VX2wFZnwqGOA31d8nxrqf3GcyAvQPsgV9Wr4oqID2ojksZRImcUK%2BjZoMOZneYgvb0FdXoVWCYpYx4s5mHvc6d%2BRgn3g6pHUj1cIu4TqgIlX6zO%2BJuGlrc1H0Yz6KyV7CdZq3TqWwGkfIaH7D32HVFu3Wq%2FZoQD7XtQ7KWz0BTqQ%2BsZI64Vi2hgEXEC3uGilVo96%2B6bNY2sI4r1WGwAeckN3MRWNfUwJ1uyiabgw8WfHwiG21SUEFXvAEfpji5AJ54jUuUCqLmnuKF%2FiohYTEjO%2BfxQuYkRX1To1fXlHNf6fHpUWhuStmcNzyirFm2C%2BAYmW38xSERh05JccD7q2UZ473ODTWOmiRhCZTc01dMhHkjq%2B0X2putWwPFvBXAd3z4CeFsiaGKoTbOcfwB6tdalHytuLFlcqQwFvOfOcevsY6kXH5wiaNUi6uQQHxrFdtkOj2WdvRm4ycK4VuiY%2B3x2pRqWCJOwyzkrENcf7MKiAm8cGOqUBCQm%2Ff7ap5hPxok5GNkHd5WlS2hX63v80t4KfP7kMqtb6DedpzoLpu3Cwt5y6b2nVtHvCcnOs8kqqCTu7vGOCsgOahKAiI5rPLNKRghOUU2NHcRxjPwGK4maUpz3DsfuKLU8H3cbnM4cPslhIVjPeAI8p%2BZPaeDjZruJOp3N6a4OQAdKeVF0OEdIKGo2BO2fbnxSmKvWBWEM%2B8AfsgKLywU2r3UY6&X-Amz-Signature=c2c9afedb47895b5af2ae447dd902e0fb7210863c49ec49aa23a4c260f54089e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


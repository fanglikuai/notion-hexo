---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVG6NOS6%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH5x9DC9KYfy1s5S5NffZD3ktlY60avi%2BAGl9xQ21B7XAiEAuuCMB5XR7nFg%2Fuf5yjOlZLNfBz0TPghpe24klcRL3RAq%2FwMIQhAAGgw2Mzc0MjMxODM4MDUiDNeeroBCswTl9qyPUCrcA0B0xuubHmqXEyGQPV1U9m4lDgHEpUpdUdqw%2Fuh9wKa2mDBXbyFPICWEeidvmviASXG4OfQ5BeTtLp2DHBUIarp24z2fBhDMc5rT7Vz6MOfSRyZ%2FutIyIqAtSk4%2FB9oPjjv3E0GpKMf%2Frua6I2%2B3qcTlsdY0O%2BxYF%2BKkBAtyQ23SQiTMnsj6ty9aGqZOhkmYuaUXYcZZAwOJTMDlRtb3Q3XynNe51Ni8rGJ92pORKnGxCIufs8wlOtkhYR6Hatiw93BYf31wtq205MNlPXTwwLEg%2BFgLnB6Vvw2cUoqkqdwQt2dDf1fwRrZ9N49kxvnumoULLp2AU6iPaAC3UJOKhqJz77lYeZTXfmEE31QL9%2ByBwqy978xdPz%2FENBqZ6Pq5tRGCY%2B0sNwdMczqC%2B3zctCuT1N%2FlPb3LNMRxIAzMUMaqafoygYVziezYa4FenJGi4%2B3UXK%2BcJuJuU8TDRYBsDA2IqXKJLRxnmQkPsam1SIpFIa3ZQt27uCMjVx5PqtSbr93W56Kc4wQ%2BFcu6uTUQYguG12vc2UwaNBjUKnHGXvK9OXPQMo2snolYxdStpTW3NzMHprMynLVRCtOzJZNNYNJwYKjrWJESF0hhCftQAvNRO3s1Hzzw3Eq053J%2FMOLR58cGOqUBrHsWy3nRYEsJqkTcB%2Fr4RuGy1yKlOBig2vEGERdWnUXYUsAgXRQSw%2FrVexB31EUIa0uksMznsYihK6iUHS9o0vgb7uIZk6DwSkpdCby1YexTKi2zjPOuVS2B2x4Ob5BMbeNU%2BWjlYWUytACzXN2oY4lc6HNSLd4a%2BtrxOJNImYeM2UTb94qVRjxLZ5117kmuk3Hi5gyvhZueGn5MBeiH%2B3Q14ZEK&X-Amz-Signature=76f5cbeeccfe358cfc71616934c253621c2f4fa894fcc8344d7350d04d53ac85&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


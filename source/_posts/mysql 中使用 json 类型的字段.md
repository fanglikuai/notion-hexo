---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VUE5342A%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJHMEUCIHEOlYTlqnM9ryNzdGfLjW9IeZDzRtBGZaolAX8p68mjAiEA3La6qxz7qkrdUoGVeVSWArjm16MqJkRXtOTNdpWCEEUqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHsU0R6P0Qriv1oCZSrcA9MK20qPijQ04u1wlHU4dB6keJk63CLxzJ14trb%2BL07VsF%2BYBbHoDPHN3QN7Gu6SivAr9hW62g8%2BlVMU9bRByM%2FmLTMqkCLQx8Wb9d606cP5dj9NdmgY3vYznE%2BuW8227gAMPjPug41nFxUEJ%2B%2Blk4G65adzOElmbvmq17dwEKZKqO%2BFYDo99Vzp6f5yt%2F2bnbDQm9nlZymT472W5RKu6IrE%2FW8rtRx%2FcRTWEF2ApRgOFsSnrSay7R6%2B8n%2FhgablpTYmm6s4DU8AFsFKC3hWIHoDm2I%2Flj8dHSRRXGGveONRQ%2BbT2GcS%2FOIqGny1mDhZW7KmS%2FxitiFPn%2FqUxhx6AHzXBkCJ55r5KnF2SJuCNM9B0WspOBCGYzu3itru6ScXgSuv3N41XO%2FxVrXrgzh7%2BEx0Sa5%2BvsdRU8fuDvxaT%2Bjar3lIa9njpxJT1%2FZ2KyTVt0gBdB%2B6MIIU4UPAqhi0updbvpZZu9jXQxESt%2FkUyxPrUQvZPnft%2BM%2B0LeixCeCkdwAH853K%2B%2F84K0XhloDRME5JnijIbeqefhqKRzKG89ElPVQo7c5undePpNkUrBnhSbc0VUo6ujPA281C%2BTos%2FJlp2Yl276fOPxlDNjA4M%2FQA8Uav36%2FoXhTiWekVMMCf2cYGOqUBhOpVDmMIAwThsuLjc5z4kG%2F1q4tBXVOT%2FGTD%2FdtZHKxSjOVjAVczkShY9X7JioayCdF80YH5GV6PGRaMy3oJZFLhDXkbTCmwPbzDrLxkJCVclE8PN2A6zmnmIROOkkgRArdUIyrvUbPhMzv3qD5NgunHofLOBv45U99eFSctGNflomfopy0kPBH4XuErH7vvFrvO7ro9vO6gZVJ%2Ff93T2SlgxBkl&X-Amz-Signature=69bd883d6b2801e25b4f0f39245549afd27cc1f2e9f2e3ee880c86f22c49bbc8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


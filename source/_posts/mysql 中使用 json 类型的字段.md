---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W75P42TO%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGmMlaf9u3hMIsQVZrLc%2B%2BcmPypr9Wl70O2eSeCDcqHqAiEA%2FFJcTMSgR9XRNtcFHZBgWCIR1t2Nw08DY5dJEXlDduEqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHGbYm4eMf5JhDrsgircAwmgdsKUfz2sByb3KMyfPO6GCHH6alXc%2Bk0b8FktR%2FMCJbHOwMDjg8uad%2FmA3qyoXknJj29Wo2HmJFkokpowwK5pMBdOyBQAlHvpZv6KyYB9PHwDc0H92m3v8aneysVxqAcIkcr6YSG%2FM0VWBiJ5jMfl2uVFCVgM1y%2BZQol4CjxTJ0KOlCx8XB7fn0uBskPG2vts8alxHcn9pwcflj%2FH0h2R6Qkwb3IQ%2BIILo%2BeaUNiwxM787oZZo5RQEt7fkp83NUo7SIrGcWn5N9JGr3%2BWK9EK6Ln%2BGPKtxptgb57vlmBriUfa6D6s96DRhCcNFeHYN3mGFD7qSL9L2WLr6hciJDgsydcT8r7PDtxyeEN4q5Yoj06Hj2ug3CEADx176KdaOl5tv5y1jWfNG3McbieSNgi8L4YglANhPItC3MN9t4r3ITY23iYXMVUOcctzJelxLImHT%2Bc32%2F5AkL4x6WiwfZlVriLiUEm6q5tj5EWA7RutOjZ0YQ4gwkA4fl98hy60E05fZBXd17vWRCSEB%2FbXBCaVBW886gOWUmKuONOcYKr5BrHvJpZ09nmi7tnx%2FQGwSb%2FYYQ%2Br4oXa9j8B0rgF%2FAZxd7KafLA0mqbjic17dT2BOtZGNkeD8J4i%2FYzWMI%2FqnMkGOqUBc6B4viJEF1yLURtxJ9ASiASwZHCAEYTCVF%2BJkisrtXzLsH5t1Vsmq46mqKhTXzyJpxrletV2R5aWEp3Q%2BQQ1GUWlFFnjHvRV7SmcBO62LLvXKN3KVI0mRlYsiSxuO7BuOrUlzvkZ3aupF8Rlz1ur7rk7tkPnTedLc9BRN5h3X2BlFv7CxbR1jqQWrlrAT3QtWSnswcDDTDAUvTj3IO5k6n9yDqhC&X-Amz-Signature=fcdb9bf1ce520ebccfbbedc4679847d71de7b7d69bc7f6cae6ab0fbdc3505dba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


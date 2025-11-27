---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RCRILRAY%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC3KSTQ6pOCVFvSdOu2dFIbX9lixFwGbJTgQmTqUY2g0wIhAKxWVc41TC%2FTdcgutkHWzaoIxL1IeBsmV7EgI%2FPqm1zvKogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy2MVrzCCl2Jo05vjIq3AOARuyLm9NBksgAvBjWUffoPdwVk8%2FyVsbk8%2BOsEE3yA5QTGGNY%2ByB7wjuDxUKr9soRlxsFESgeH4LBbf%2BNbyTh0xR%2FUjsao5zy8MRZi%2FG3C5rb29VSfJzJTjZ9IujM8ch%2B5%2FoaZKS%2FIEKjvmpfdVjfx%2FcLsF7cXETwqXay9yJzjL3rRA9CwOsihJxcf4oVPgNHGK1AX%2B7ISHL64QbAz4LuXPeWxhAx0KYiCfOOcLYgt7l6qqJKOcJLsDZkyYssj32LjMQaF23y47W6jtX4PcJQUavDk8yhAYT%2B5r5beKjhUcGDG3KU%2BwGZXYX62Qji2Y4osOd7znjN3ctaEK8yOgIpCFIG8lfJeRgbjMYk0uMdpQvkl72WSxr7BASMkx2oahgAx4hI%2BtvxFgp9QRPvxmqLGeu%2BhMtvfAg6TOgbCuCRJOahjmY2zyuP8VnOlW6Hh57TGGpLaDEF9rPtPDbw8I%2FyASGXkouBKurkGILT7bLH%2FeYv1HdVhFZPs9s%2BoFPXHgCatXQB79WDhuCGLNfGDGTIzJIFGeLcvxCu3gznpjnTHIWvUs22J4IvQoJZQQ7TO7DlX7P8WqpGwQLGv7Y5yAq18SHeCBBrmCIZBnosFKDdPOk2rci1E5xaptp1szD8wqLJBjqkARcWeUT4zWmXim795npL%2FVOcZsr%2BW0WDivtQyqA4x6iWiOZ3Rlgfan4r2cmTNyLAWesUuufpWkc1I3VyHkHV1iPI1InaKHHjH98UEohXxwV1htA4IyyKtbB%2FBSSsjjWJth6iBucprqBS8u3uP7M4cddAeoGLJfY7DucU1Ih3PvMndxWZYJc2GDQa2daBlWiMWxQJE0QsS%2Bo%2FwsznbiXhskEhcyPj&X-Amz-Signature=4516db31f414587cfeb5bee8192c57b7260421a9cdc0589b61222206b4d60b5f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


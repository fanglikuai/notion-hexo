---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMJ73YHT%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDA4Lb53K1hwd6Eo6Z5YbKBtH5n%2BZlVA3XEttHRsNCbnQIgX0GcWCI3%2BfvhYJ49mwzvc00RbHF317pIP6fkRafwRx4q%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDCMI%2BaCR74O0mCUL3SrcA6M2F7o259cGLLc6884U%2FjPfifTOc8aV1sBd9tkNGZJS40IJWH3NGFxc5P8caWb8IeQitTE2oI1t1N1GvPhtvif%2BhYc06KNoFsCuJIm5SVg43Dc0xJLoe3HcD0t6a8RG29vHnkLG286KQfjizRfo8hiiAJEgWSDPEuhWVdL21nbUP49e%2BQXvhHDJhmPt0E3JY4hSPIzALmgJ87m228zFTE9IyTApWbYkb69xp18KRt8e3dWu9q%2B8So5DIjW9%2Fy47O6GDY2yFf%2BpybRw5hibAIyevTPO0lkfkskiHxf1V1X%2BxleipGPCG3y0hPJTgOq%2F7Xgr8LUEeb9FJArwRemv3abT98IhoSusllvjO51MKun6KGk21zSXkRKVo6xvvAevQx%2FCiKG%2BYyUtgho2oXnUnB3cWlCMckgI0AU1ggz7xRAfBhZigrCGhdLjy2WD5qUJS2Yr9Mxg%2FyrukTRtMisnsOesN4cGuJ1rVZsns8M9A0OKftXIfP3q3RxMCYgNIqTPiTu4yky1ryxJhNF0S46etmF36A6jAZu6PDDrmEnioasD4jbTS%2F%2BoNCzH0vi6pTLWL66rKy%2BhZn8vOEa2JjDQrMIo2DB%2Fdt05APpfb2JvLeQr7FbcEuX8s8es8Ifu2MLecg8cGOqUB7Vr%2FLE%2BDAkO42QqEmutV9duwF4hvBnAi0ZXUpbjf6LQmJg6XqE4Xitq5mRT4DsQO%2FMWCioW4jI8x%2F0DrDJe1D4dEXri%2BOg2PtIcNZp7b1%2F5DuJKZWpIp5zQb219tzJqFvqGLOFFwj7GqT0e44O0mxBpcIgThVaUFlPpnM9arH5aut2xz3soM0yyvEnaWJjMXATBNhRmm1ieHD%2Fa32eBYgBBHRgNs&X-Amz-Signature=12d55ef6cec329ac0de61bf56658ddc62a9e064b7fad641152f2d96ed6aacefa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


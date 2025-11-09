---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665LOI337P%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQC%2FEWawZekO4cSy14Xfl0fdnGO%2B5nHLGKYbFWtNYKVs3AIgWEALIjHN9m96DDKlCSa8r4IWNNPWp%2Bqyf5BPtZIpTggqiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLYbzgdCCxkUSnKQQSrcA%2FtofZSPGcIYe9HgoUvk9EQ3A%2FMP2%2B95YCkpOrlCFB71PCPC8dgb0VAE65invOibAVRUmTDBNADYzq5H7yPFx23QE2w4LzkeSrcPTJ%2FLA%2B9nyUuHvk3i4k7CTAiksiisnzdZfJt9IMcCzGPBwivFkGpZP1ms4jeAiKSXHOR86IkQKvaS5MFoj%2FFbqVMhmSjkX%2B1y%2BamOA4oqrNQGwjgLZDlfPIsQIg9CJ7IHk%2F8jslBumVt15%2B79gGxyRy%2BnqXOJYQ3TYIOeffjJV%2BeP1DSkMHMriyUb9iJ6svP1rSbMViHGqKwbbFDOKQXGBygAZz53ZbxMZ5Z1MTMz2%2FfQ3UhSpsC3mjxYitR3BxUzHTXmGsdFHTCdtiCpvWqlyxnxk7LEi4cboesoAXxblhoOQxAX7libEnAb9ap0%2FMw8rSLJzIhSvGfaRTnkWSRiE7Sww1AL%2B5tFP3C5nqqRNkSy7Obmb602xlBczd4sv8WYogaHup5vKun1Z6SQH6WwUF8tDKxhHygieqD8WVuv%2FSKjUmFeLtR8hksOIt3z6NQaGkFEsOXSFiNBUOZN6WK%2FejqU%2FaqCwGq%2FlgQjmWi%2FAGWpBFoS04umc0rzYty2dcijr0%2BLrthnM8zP6G6pddHaqBJUMIDuv8gGOqUBsuYFmSqsPaflKCQ1A6fd4OT1D7xVgQtYuL8apPRqOYIJMZYNrcjOB8HAGJocegsNcETTYjglwgdOHvcl42I5PdyipOCgV0gUOf%2FOKc6Yu%2BguRsSU6W%2Bvs1Ttb1eS%2F%2Bbla3QTiX2xReto%2BSzhJVdfX2eTKo0fvQ8QO%2B%2BknbgzQdQ4WQH5Xh6zSXIqtf9Q8Pb8Eky1Lmrv0UMCePghxKBY9IaS5R7A&X-Amz-Signature=1e4db8fbef5b90424c5e76e6c9b3d6a2f89f26290664146f59117768e7805733&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


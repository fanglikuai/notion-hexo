---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46663R7PB55%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIDks87EZcuweVxV7xvtsd%2F8UII%2Brj%2BPecLZcS%2FYel2q1AiEAjtCLBqPWDKnQ5VDMG44t594VoiMVWOh401w0RertUQQq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDBizeTBRO%2BLXlkX05yrcA7T7izi%2FRL0lg5EEC0PYvzZMCcPzTVbdNRO3JuFmRbCeXuiK1X0M5m46upeLMT9mc9cgXkRF78sRWGdAI%2FpB1hQ4yma%2B9N%2FLElTE6XqdUkeBznAUJyrx9HYCYyuBIZrjXl%2Bx51LwDuni5Pi9Jj5%2Fz42ACI9whnjaTlsfZz0Q3XZ%2FUQTEqOApRaae9fhz%2FDUm5Ji5fGTYSvFMEHTQHUhZ5gxcfwkoC7Qsd2KnztvKWXzB4gZc3zwVZDPz67ROnUhXEkSHGV%2Bok42wdC2OsLBkArCygSf6eKKWOiLV09IvGK0aGFu6dU%2Bstt85HfRbpND%2ByY9MTml3%2F4%2B%2BaXXEBziuvvDO76CeqFaYBEqj%2BlBBsqUbyvkS%2BmoweBIsDO3naLx%2FofsJR7geWuCdNIODfUtAwX1vSbyQlhHSFQEjBYjb5RHRDEkXXLnsD5SeMRGhLdwj2MqCJgrzUSzmRz%2B1hgvAzt3WvnK9V0ILr7GUPOiM41S%2F0XJvQ9QyfiMPp80L2S3562qhhiX%2BX9p3eJ5AM692eLBqneWt4GBRMJF%2Bej9kcBCVVa9ogo7l2YHUS2uLk7Np5hEILj0PjQkLv%2FLy0tbRFn6NhAbAWkXxlRhRicYSe2Z%2B3ZglFtsgylVFkpQ3MN2mq8cGOqUBwyZIXH%2FRDQCVG%2B4DsfF3E%2BWvWtwzBmEX8gclL9slLzAro0cdmzBSmnJkiqNqe9u4QRCT8YJuFVB65%2FZd0F8Id0tddiw3M4CacI89woYpiVrgr9G0CrP6e6F7kl2Rkf%2Bs2WNyIPaaP4ej1Wp5aIeZ5Ak3S8i6iqBhpEtMWzSKSg1jtToXdY8fYA7PO7LWJ%2FC5rprH0M1fzLxJUIYMpNNdsdaTmFUG&X-Amz-Signature=fbba30dd86d68c38240616b7aa98167feff168c07c0763e3ceed741ad41660e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


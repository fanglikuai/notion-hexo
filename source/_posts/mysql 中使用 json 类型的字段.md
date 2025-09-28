---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TQKZFBQI%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T100037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC8aCXVzLXdlc3QtMiJHMEUCIH4iK70Vkykl83iETkgdZ4uXC%2B5FUYryGgeCrljAmVOmAiEA8BtOKrt9CgwveOefzbteHajwiKVDb%2FMEDlRtJBq%2FEFQqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKCr7d%2B2p6Fo%2FFPc%2FCrcA9KTR%2B8KtS0uJuEGDOXE%2FV5Fn0lIcmsaO9d6RHLbFffx0GOMM4MqnfxK%2BrHyg37KZBoy5ro3ZyK%2BARqam%2BSIb%2FrhDcdMAfYoRt9edQ3nhO2hGZQbC2JIkKe7QspGpMoJEU5wY3SMjGU0EZgLKOOtUC3lnjovTTs3cJIN0kOnrMv1uitEDsAjxED8gxpcOqUdf6U%2FILVAds3bOh5EcmBkyy5BWPA7yU4jJy5CPjXh10w9zJ8BZPj4dHio73fd%2BRupgxETcxMR%2BXVt9NyMgBWN2xeo7Rm4HwVxKMr5EhXnrPUG4g25xxrm7bkYmOWAnQNR8OYTbHYFPsED979%2FoYoinQELTFNwfgY6aS2cDbUVFPeSeEc6OGGJ3DyivtLqOYwdhEoJEk3lIlv3HAFrJeaA1wc4OjABzzuH1IMDG8sS9wOzXF16rBWtxdTAktoQtHnrAE2do8NvaUEO8KYJGdFDcjmmLkSMCRD7NNrHyhRdO2RO%2BzgClpKsnPqQYrh6x4I8CwCfebXgDQ%2B4D8xARPt9wOBTLuwg2vFGFyMb1UosY%2Bm4TwXLAGjrTF6xDBRsFKCLfBz0GhlgvgO3sat68%2FbQART%2FpQ0lXwdvTiHWoe1kXC2T4Sozzj4COC%2BkpOWPMLa648YGOqUBJQD7wVeIQ2hmKjddm5KIqyf2v4JXkZ8%2BbfB7nqdhH5Z%2B0n5w0gV3ku%2BUNOzebFuIY0veapNQPzK6sV7TFQz0IX5ThB%2B1BOawnm%2BL6TdS7eD0uCCxy2orUDZSKIzX7ewROwOBEotHAMXvtE6Wx0CwFtG%2BZdKst1QdVGXAD8d4%2FblLxxE3BDAPqhnHuAzhR1Z1AIwH%2Fsj7C%2BPTjEQnNn78cm85rPU%2B&X-Amz-Signature=563063868598d533237c40432abfaa3aba180f95d032734b24971f7358c6aedb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

